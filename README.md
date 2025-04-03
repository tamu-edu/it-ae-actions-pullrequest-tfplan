# it-ae-actions-pullrequest-tfplan
A GitHub composite action for generating and posting a terraform plan to a pull request. It will post the output of `terraform init` and `terraform plan` to the pull request found in the `github` workflow variable. It will also upload the terraform plan file to the repository as an artifact intended to be used with `terraform apply` later.

## Inputs

| Name | Description | Default | Required |
|------|-------------|---------|:--------:|
| debug | Debug workflow with tmate if an error occurs | `false` | No |
| GITHUB_TOKEN | GitHub token for access to the pull request |  | Yes |
| save-artifact | Save the terraform plan as an artifact (May contain sensitive data) | `false` | No |
| working-directory | Working directory for the `run` actions | `""` | No |
| terraform-version | Version of terraform to install | latest | No |
| terraform-workspace | Terraform workspace to select. Must already exist  | default | No |
| terraform-init-flags | CLI flags to use with terraform init | `""` | No |
| terraform-plan-flags | CLI flags to use with terraform plan | `""` | No |
| workflow-artifact-name | Provides a unique name to append to the plan artifact attached to this workflow run. Default tfplan | "tfplan" | No |

## Outputs

| Name | Description |
|------|-------------|
| plan | The combined stdout and stderr output of `terraform plan` |
