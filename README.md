## Terraform Module for Firewall Rule Management

This Terraform module automates the creation and management of firewall rules within your Google Cloud Platform (GCP) project, providing granular control over network traffic flow.

## Table of Contents

- [Overview](#overview)
- [Variables](#variables)
- [Outputs](#outputs)
- [Usage](#usage)
    - [Module Example](#module-example)
- [Cleanup](#cleanup)
- [License](#license)
- [Author](#author)

### Overview

This module offers a comprehensive solution for defining and deploying firewall rules to your GCP network. It enables you to specify rules with detailed options, ensuring security and control over incoming and outgoing traffic.

### Variables

This module provides various variables for customizing firewall rules:

#### Required Variables

* `firewall_rules`: This list defines individual firewall rules. Each rule is an object with properties like:
    * `name`: (Optional) A descriptive name for the rule.
    * `action`: (String) The action to apply to traffic (allow or deny).
    * `direction`: (Optional, defaults to "INGRESS") Specifies whether the rule applies to incoming (INGRESS) or outgoing (EGRESS) traffic.
    * `sources`: (Optional) List of source IP addresses or CIDR ranges for the rule.
    * `targets`: (Optional) List of target tags associated with compute resources.
    * `rules`: (List) Defines specific traffic protocols and ports allowed by the rule. Each rule object within the list specifies:
        * `protocol`: (String) The network protocol (e.g., "tcp", "udp").
        * `ports`: (Optional) List of ports or port ranges allowed by the rule (e.g., ["80", "443-444"]).

For detailed explanations of all properties and validation rules, refer to the `variables.tf` file.


#### Other Important Variables

* `project_id`: (Optional, defaults to null) The GCP project ID where the network resides. 
* `network`: (Optional, defaults to null) The name of the network these rules apply to.

**Optional Variables:**

* `use_legacy_naming`: (Optional, defaults to false) Toggle to use legacy naming conventions for firewall rules.
* `include_implicit_addresses`: (Optional, defaults to true) Controls whether to include the default source address range (0.0.0.0/0) for rules without explicit source or destination specifications.
* `override_dynamic_naming`: (Optional, defaults to specific object configuration) Defines which attributes to include when generating dynamic names for firewall rules (not applicable with legacy naming).
* `prefix`: (Optional, defaults to null) Prefix tag for firewall rules (used for dynamic name generation).
* `environment`: (Optional, defaults to null) Environment tag for firewall rules (used for dynamic name generation).

### Outputs

The module does not currently provide any outputs.


### Usage

#### Module Example

To utilize this module in your Terraform configuration, reference it as follows:

```hcl
module "firewall_rules" {
  source  = "github.com/your-organization/firewall-rules"
  
  firewall_rules = [
    {
      name        = "allow_ssh"
      action      = "allow"
      direction   = "INGRESS"
      sources     = ["0.0.0.0/0"]
      targets     = ["ssh"]
      rules       = [
        {
          protocol = "tcp"
          ports    = ["22"]
        }
      ]
    },
    {
      name        = "allow_http"
      action      = "allow"
      direction   = "INGRESS"
      sources     = ["0.0.0.0/0"]
      targets     = ["web"]
      rules       = [
        {
          protocol = "tcp"
          ports    = ["80"]
        }
      ]
    }
  ]
  
  # Other variables as needed (project_id, network, etc.)
}
```

**Standalone Usage:**

1. Clone the repository containing the module.
2. Initialize Terraform within the module directory.
3. Define the `firewall_rules` list and other necessary variables in a `terraform.tfvars` file.
4. Run `terraform apply` to create the firewall rules.

### Cleanup

To remove the firewall rules created by this module, execute:

```bash
terraform destroy
```

Confirm with `yes` to proceed with deletion.

### License

This project is licensed under the MIT License. See the LICENSE file for details.

### Author

- **Leonardo Issamu** - Initial work and Terraform configuration.