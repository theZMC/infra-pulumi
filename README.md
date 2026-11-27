# Infrastructure Repository

> [!IMPORTANT]
> **Retired and archived.** Nothing here is live. Everything these stacks managed (DNS in Cloudflare
> and the GitHub Actions role in AWS) is now managed with OpenTofu and Atmos in theZMC/infra
> (private). The Pulumi stacks and their state backend are gone, so `pulumi up` has nothing to run
> against, and the secrets in the `Pulumi.prod.yaml` files can't be decrypted any more. The code stays
> as a record of what Pulumi managed; the rest of this README describes the repository as it worked
> before it was retired.

This repository manages infrastructure using Pulumi with TypeScript.

## Structure

```
.
├── .github/workflows/    # CI/CD automation
├── stacks/               # Infrastructure stacks
│   ├── abscond.io/       # abscond.io domain infrastructure
│   ├── binari.ly/        # binari.ly domain infrastructure
│   ├── curlto.me/        # curlto.me domain infrastructure
│   ├── callahan.house/   # callahan.house domain infrastructure
│   └── zach.media/       # zach.media domain infrastructure
│   └── zmc.dev/          # zmc.dev domain infrastructure
```

## Workflow

### Pull Requests

- Creates a Pulumi preview for any changed stacks
- Posts the preview as a comment on the PR
- No changes are applied until merge

### Main Branch

- Automatically applies infrastructure changes for modified stacks
- Uses AWS OIDC for authentication
- Only runs on stacks that have changes since the last commit

## Stack Organization

Each stack manages DNS and related infrastructure for a specific domain:

- Configuration is in `Pulumi.yaml` and `Pulumi.prod.yaml`
- DNS records are organized by type (email, web, etc.)
- Secrets are encrypted using AWS KMS
