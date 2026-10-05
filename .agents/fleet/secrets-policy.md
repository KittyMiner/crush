# Fleet Secrets Management Policy

This policy governs how secrets, credentials, tokens, and sensitive values are handled across the entire fleet orchestration stack.

## Core Rule

**No secret may ever be visible in code, prompts, logs, reports, artifacts, or any operational output.**

Secrets are treated as privileged operational data. They are:
- generated and stored outside the fleet stack
- referenced only by safe identifiers or environment variable names
- masked from all logs and reports
- validated through indirect access patterns
- audited for exposure incidents

## Secret Categories

### Tier 1: Operational Secrets (Highest Risk)

- API tokens and bearer tokens
- Database credentials
- Private keys and certificates
- OAuth tokens and refresh tokens
- Service account credentials
- Cloud provider access keys
- Encryption keys

**Handling**: Store exclusively in GitHub Actions secrets, CI/CD vaults, or HashiCorp Vault. Never touch the repository or workflow files directly.

### Tier 2: Configuration Secrets (High Risk)

- Provider API endpoints with auth
- Database connection strings with passwords
- SSH private keys
- Webhook signing secrets
- API key pairs

**Handling**: Store in environment-specific secret files, excluded from version control via `.gitignore`. Reference via environment variable expansion only.

### Tier 3: Operational Identifiers (Medium Risk)

- Service account names
- Role ARNs or identities
- Project IDs or account numbers
- Domain names tied to restricted access

**Handling**: May appear in configuration if anonymized or if the identifier itself confers no access. Audit regularly.

## Agent-Level Enforcement

Each agent in the fleet must conform to these rules:

### Fleet Orchestrator

- Never hardcode credentials in mission briefs or instructions.
- When delegating to specialists, refer to "the provider credential" not the token itself.
- Mask secret references in all output.
- If a mission requires credential rotation, escalate to Tier 4 (Executive/Policy) before action.

### Calibration Specialist

- Baseline metrics must never include raw credentials or tokens.
- If calibration requires authenticated API access, use pre-configured connections only.
- Report calibration results without exposing the auth mechanism.
- Flag if credentials appear anywhere in baseline data.

### Optimization Specialist

- Optimization tuning never touches authentication or secret rotation.
- If optimization involves credential caching or reuse patterns, escalate for security review.
- Report improvements without revealing internal token or key strategies.

### Coordination Specialist

- Lock and lease management must use credential-neutral patterns.
- Service discovery must not leak API keys or tokens in messages.
- Consensus and state sync must not include secret values.
- If credentials are part of coordination state, store them in a separate secrets vault and reference by ID only.

### Health Specialist

- Health checks must use authenticated connections when needed, but never log the credentials.
- Mask auth tokens from all health reports and alert messages.
- If health degradation is caused by credential expiry, report "authentication signal degraded," not the token itself.
- Recovery procedures must never require manual credential entry in logs.

### Compliance Specialist

- Audit policy and access control without exposing the secrets being controlled.
- Compliance reports must refer to "secret-X," "credential-Y," or "provider-Z" rather than actual values.
- Traceability must include who accessed what, not what the secret was.
- If a secret is exposed during audit, trigger an immediate incident escalation.
- Never include secret samples or test credentials in compliance examples.

### Research Specialist

- Root-cause investigation may require tracing auth flows, but never output actual tokens.
- Use session IDs, credential IDs, or anonymized references.
- If the research reveals a secret was exposed, immediately escalate and do not continue analysis.
- Hypothesis testing must use mock credentials or credential-free simulation when possible.

### Reporting Specialist

- Final reports must never include actual credentials, API keys, or tokens.
- Redact all auth-related findings to references like "provider credential," "API token," or "service account."
- If a report would require showing a credential to prove the finding, use a hash, ID, or timestamp instead.
- Executive summary must treat credential security as a first-class finding without exposing values.

## GitHub Actions Standard

All fleet workflows must follow this standard:

### Secret Storage

```yaml
env:
  # ✓ CORRECT: Reference via secrets
  API_TOKEN: ${{ secrets.PROVIDER_API_TOKEN }}
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
  
  # ✗ WRONG: Never hardcode
  # API_TOKEN: sk-1234567890abcdef  # BAD
```

### Secret Masking

```yaml
- name: Add secret to GitHub Actions log masking
  run: |
    # Use GitHub Actions masking to hide sensitive output
    echo "::add-mask::${API_TOKEN}"
```

### Artifact Handling

```yaml
- name: Generate Report (Secrets Safe)
  run: |
    # Generate report without including secrets
    go run ./cmd/fleet report \
      --output report.json \
      --exclude-credentials
    
- name: Upload Artifact
  uses: actions/upload-artifact@v3
  with:
    name: fleet-report
    path: report.json
    # ✓ Safe: no credentials in the artifact
```

### Secret Rotation

```yaml
- name: Validate Secret Rotation
  if: github.event_name == 'schedule'  # Daily rotation check
  run: |
    # Check if credentials are still valid
    # Do not print the credentials
    go run ./cmd/fleet validate-secrets \
      --check-expiry \
      --mask-output
```

## Environment Variable Expansion

For local development and project-level config:

```bash
# .envrc (never commit)
export PROVIDER_API_TOKEN="sk-your-actual-token"
export DB_PASSWORD="your-actual-password"

# .gitignore (enforce)
.env
.env.local
*.key
*.pem
```

## Secrets in Agent Prompts

Agent skill instructions must use placeholders:

```markdown
# ✓ CORRECT: Use placeholders
When validating authentication, check that the provided `${PROVIDER_API_TOKEN}` 
is valid by calling the provider's status endpoint.

# ✗ WRONG: Never use real values
When validating authentication, check that "sk-1234567890abcdef" is valid.
```

## Incident Response: Suspected Secret Exposure

If a secret is suspected to be exposed:

1. **Immediately stop the operation** — halt all related services or workflows.
2. **Isolate the exposure** — determine the scope and timing.
3. **Escalate to Tier 4** — executive/policy level. Do not continue diagnostics.
4. **Rotate the credential** — via the secret manager, never in response logs.
5. **Audit access logs** — check who accessed the exposed credential and when.
6. **Report without values** — "Credential ID X was exposed on timestamp Y. Rotation completed at timestamp Z."
7. **Do not commit fixes** — secrets are never part of the fix path.

## Validation Checklist

Before any mission, handoff, or report:

- [ ] No hardcoded tokens, keys, or passwords in code or config.
- [ ] No secrets in workflow environment or output.
- [ ] All agent instructions use placeholder names only.
- [ ] All reports refer to "the credential" or "credential ID" not the value.
- [ ] GitHub Actions secrets are used for CI/CD operations.
- [ ] Local .env files are in .gitignore.
- [ ] Audit logs do not capture or display raw secrets.
- [ ] If secrets appear in logs, masking rules are in place.
- [ ] Secrets rotation is automated and tested without exposing values.

## Compliance Audit

Regularly audit for secret leaks:

```bash
# Scan repo for common secret patterns (local only)
git log --all --full-history -S 'sk-' -- '*.go'
git log --all --full-history -S 'BEGIN PRIVATE KEY' -- '*.go'
grep -r 'password.*=' . --exclude-dir=.git
```

Never commit these scans. Use a local script and review manually.

## Exception Process

If a mission absolutely requires working with a real credential (e.g., secret rotation testing):

1. Escalate to Tier 4 (Executive/Policy).
2. Use a short-lived, limited-scope token or key.
3. Use a dedicated, isolated environment (not production).
4. Mask all output and logs.
5. Destroy the credential immediately after validation.
6. Document the exception in audit logs (without the secret value).
7. Do not commit or share the credential.

## Training & Enforcement

Every team member working with the fleet must:

- Acknowledge this policy before first commit.
- Understand that secrets are never acceptable in any repo-visible location.
- Know how to escalate if uncertain.
- Report suspected exposures immediately.
- Review secrets policy quarterly.

## Policy Governance

This policy is enforced via:

- Pre-commit hooks (block common secret patterns)
- GitHub push rules (reject commits with detected secrets)
- Automated secret scanning in CI/CD
- Regular audit by the compliance specialist
- Incident response procedures

