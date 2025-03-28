# it-ae-actions-pullrequest-tfplan
A GitHub composite action for generating and posting a terraform plan to a pull request. It will post the output of `terraform init` and `terraform plan` to the pull request found in the `github` workflow variable. It will also upload the terraform plan file to the repository as an artifact intended to be used with `terraform apply` later.

## Inputs

| Name | Description | Default | Required |
|------|-------------|---------|:--------:|
| debug | Enable `tmate.io` debugging if a failure occurs | `false` | No |
| save-artifact | Save terraform plan as artifact in Github Action | `false` | No |

## Outputs

| Name | Description |
|------|-------------|
| plan | The combined stdout and stderr output of `terraform plan` |
