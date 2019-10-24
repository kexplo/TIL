# Error: No valid credential sources found for AWS Provider.

Add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` Environment variables to workspace's variables page.

You must check the `Sensitive` checkbox.

# Remote backend configuration

It is required only if you are using the CLI driven Run workflow.

`terraform.tf`:

```terraform
terraform {
  backend "remote" {
    organization = "<org name>"

    workspaces {
      name = "<workspace name>"
    }
  }
}
```

`$HOME/.terraformrc`:

```rc
credentials "app.terraform.io" {
  token = "<terraform cloud user token>"
}
```

references:

- https://www.terraform.io/docs/cloud/run/cli.html
- https://www.terraform.io/docs/commands/cli-config.html#credentials
