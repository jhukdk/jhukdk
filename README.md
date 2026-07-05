# David J. Kim

**Cloud Security Engineer — "Security Controls are Production Imperatives"**

CISSP · GWAPT · GCIH · GSEC · Security+ | Current DoD Secret clearance | Writing at [jhuk.tech](https://jhuk.tech)

Each of my projects adheres to the same rules I would hold production code to: Infrastructure as code, changes shipped through reviewed pull requests, keyless CI/CD via GitHub OIDC (no stored AWS credentials anywhere), and test scripts that prove the controls actually block what they claim to.

## My Projects — One Interconnected Stack 

My repos comprise a live system in Production: my blog runs in blog-migration, the WAF layer defends it, and Splunk ingests its logs. 

```
                    ┌──────────────────────────────────────────────┐
 waf-as-code ──────▶│  WAFv2 rules · attack-sim tests · GitOps     │
                    └──────────────────┬───────────────────────────┘
                                       ▼
 blog-migration ───▶  Hugo → S3 → CloudFront → jhuk.tech (live)
                                       │ access logs
                                       ▼
 splunk-enterprise ▶  S3 → SNS → SQS → Splunk on EC2 (index=cloudfront)
                                       ▲
 waf-automation ───▶  Boto3 lifecycle changes against the production Web ACL
```

| Repo | What it demonstrates |
|---|---|
| [**waf-as-code**](https://github.com/jhukdk/waf-as-code) | AWS WAFv2 managed entirely as code: managed + custom rules, rate limiting, full logging, an automated attack-simulation test suite, and a plan-on-PR / apply-on-merge GitOps pipeline. Includes tuning a noisy managed rule and a case where the tests caught a real coverage gap. |
| [**blog-migration**](https://github.com/jhukdk/blog-migration) | The production infrastructure serving [jhuk.tech](https://jhuk.tech): Hugo static site on private S3 behind CloudFront (OAC), edge WAFv2 web ACL, ACM, and an OIDC-authenticated GitHub Actions deploy pipeline. Migrated from WordPress/cPanel. |
| [**splunk-enterprise-integration**](https://github.com/jhukdk/splunk-enterprise-integration) | Log ingestion pipeline as IaC: CloudFront access logs → S3 → SNS → SQS → Splunk Enterprise (Docker on EC2, EBS-persisted, IAM instance role — zero static keys). WAF and CloudTrail ingestion on the roadmap. |
| [**aws-waf-automation-boto3**](https://github.com/jhukdk/python-tutor-for-boto3awswaf) | WAFv2 automation and traffic analysis in Python/Boto3, executed against live infrastructure: Web ACL inventory, sampled-request and CloudWatch metric analysis, and lock-safe IP-set lifecycle changes (`get → modify → update(LockToken)`) on a production web ACL. |

## Recurring themes

- **GitOps for security config** — every rule change is a reviewed PR with a posted `terraform plan`; merge applies it.
- **Keyless cloud auth** — GitHub Actions assumes a repo-scoped IAM role via OIDC; no long-lived AWS keys exist in any repo.
- **Controls that prove themselves** — attack-simulation suites verify blocks (SQLi payloads, custom headers, rate floods) instead of trusting configuration.
- **Least privilege by default** — scoped IAM roles for CI, instance roles instead of credentials, private buckets behind OAC.

## Background

Security solutions architect at Akamai (cloud/edge security), ISSO and RMF compliance work supporting the Defense Health Agency, and hands-on AWS security engineering. Offensive side kept sharp on Hack The Box. Longer-form write-ups at [jhuk.tech](https://jhuk.tech).

📫 Reach me via [jhukdk@gmail.com](mailto:jhukdk@gmail.com) or [LinkedIn](https://www.linkedin.com/in/jhuktech).
