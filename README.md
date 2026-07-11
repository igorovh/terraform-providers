# Terraform Providers and Modules

This is the main workspace for independently usable Terraform components. It is not itself a Terraform module; use one of the component directories below.

## Components

| Component | Type | Purpose |
| --- | --- | --- |
| [`terraform-k3s-vps-wg`](terraform-k3s-vps-wg/) | Provider | Installs and manages a K3s cluster on existing Debian or Ubuntu VPS nodes through SSH, with optional WireGuard, UFW, and Helm addons. |
| [`terraform-cloudflare-zero-trust`](terraform-cloudflare-zero-trust/) | Module | Manages Cloudflare Zero Trust Access applications, groups, policies, identity providers, and an optional GitHub/Discord identity-broker Worker. |

## Usage

Choose the component that manages the infrastructure you need and use it from a separate Terraform configuration.

### K3s on VPS

Install the provider from the Terraform Registry and declare a `vpsk3s_cluster` resource:

```hcl
terraform {
  required_providers {
    vpsk3s = {
      source  = "igorovh/k3s-vps-wg"
      version = "0.1.0"
    }
  }
}
```

See the [provider README](terraform-k3s-vps-wg/README.md), [examples](terraform-k3s-vps-wg/examples/), and [resource reference](terraform-k3s-vps-wg/docs/resources/cluster.md).

### Cloudflare Zero Trust

Import the module by its tagged Git source:

```hcl
module "zero_trust" {
  source = "git::https://github.com/igorovh/terraform-cloudflare-zero-trust.git?ref=v0.1.0"

  cloudflare_account_id = var.cloudflare_account_id
}
```

See the [module README](terraform-cloudflare-zero-trust/README.md), [examples](terraform-cloudflare-zero-trust/examples/), and [Identity Broker documentation](terraform-cloudflare-zero-trust/identity-broker/README.md).
