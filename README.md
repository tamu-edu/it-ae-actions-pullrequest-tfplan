# it-ae-actions-pullrequest-tfplan
A GitHub composite action for generating and posting a terraform plan to a pull request. It will post the output of `terraform init` and `terraform plan` to the pull request found in the `github` workflow variable. It will also upload the terraform plan file to the repository as an artifact intended to be used with `terraform apply` later.

> [!IMPORTANT]
> Note on private repositories: If the included `terraform init` needs to download a module from a private github repository, see below.

## Inputs

| Name | Description | Default | Required |
|------|-------------|---------|:--------:|
| debug | Debug workflow with tmate if an error occurs | `false` | No |
| GITHUB_TOKEN | GitHub token for access to the pull request (see note about private repositories) |  | Yes |
| save-artifact | Save the terraform plan as an artifact (May contain sensitive data) | `false` | No |
| working-directory | Working directory for the `run` actions | `""` | No |
| terraform-version | Version of terraform to install | latest | No |
| terraform-workspace | Terraform workspace to select. Must already exist  | default | No |
| terraform-init-flags | CLI flags to use with terraform init | `""` | No |
| terraform-plan-flags | CLI flags to use with terraform plan | `""` | No |
| workflow-artifact-name | Provides a unique name to append to the plan artifact attached to this workflow run | "tfplan" | No |

## Outputs

| Name | Description |
|------|-------------|
| plan | The combined stdout and stderr output of `terraform plan` |

## Private Repositories

If `terraform init` needs to download a module from a private github repository, merely passing `GITHUB_ACTION` to this Action is not sufficient. Instead, the calling action needs to change the source URL to embed the oauth2 token in a git-config step like:
```
      - name: Allow terraform to clone from private repo
        run: |
          git config --global url."https://oauth2:${{ secrets.GITHUB_TOKEN }}@github.com".insteadOf https://github.com
```

for the corresponding Terraform module invocation like:

```
module "my_module" {
  source = "github.com/my-org/my-repo.git//deep/path/to/module?ref=v0.1"
```
