# Terraform Providers Workspace

## Repository Strategy
- Prefer several small Terraform repositories/directories over one large monorepo when the components have different lifecycles or ownership.
- Treat each top-level directory as an independent Terraform repo unless a local `AGENTS.md` says otherwise.
- Keep providers and modules separate when they solve different problems or are imported independently.
- Do not force shared versioning across unrelated Terraform components.

## Importability
- Optimize every repo/directory for easy Terraform import from Git.
- Each importable component should have a stable source path and tagged releases.
- Document the expected `source` value in that component's `README.md`.
- Prefer examples like:

```hcl
module "example" {
  source = "git::https://github.com/<org>/<repo>.git?ref=v0.1.0"
}
```

- If a repository contains multiple modules, import subdirectories explicitly:

```hcl
module "example" {
  source = "git::https://github.com/<org>/<repo>.git//modules/<name>?ref=v0.1.0"
}
```

## Providers vs Modules
- Create a Terraform provider only when Terraform needs to manage an API or behavior that existing providers cannot express well.
- Create a Terraform module when composing existing providers into a reusable infrastructure pattern.
- Kubernetes secrets management should usually start as a module using existing providers such as `kubernetes`, `helm`, `vault`, or external-secrets tooling.
- Zero trust management should usually start as a module if it composes existing providers such as `cloudflare`, `tailscale`, `vault`, or identity-provider resources.
- Promote a module idea into a custom provider only when there is a real API boundary or lifecycle that Terraform cannot model cleanly with existing providers.

## Local AGENTS.md Files
- This file is the workspace-level default.
- A top-level repo/directory may define its own `AGENTS.md` with more specific rules.
- When working inside a repo/directory with a local `AGENTS.md`, follow the local file first and this file second.
- Do not edit unrelated repos/directories while working on one component unless explicitly asked.

## Expected Component Shape
- Each repo/directory should be usable on its own.
- Include a `README.md` with purpose, inputs, outputs, examples, and import instructions.
- Include `examples/` for realistic Terraform usage.
- Include tests or validation commands where practical.
- Keep generated Terraform artifacts, state files, `.terraform/`, local tfvars, and provider binaries out of version control.
