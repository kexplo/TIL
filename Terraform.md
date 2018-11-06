## The `aws_iam_policy_attachment` resource can only be used once **PER** policy resource, as the resource manages all of the role attachments for that IAM Policy. 

The `aws_iam_policy_attachment` resource can only be used once **PER** policy resource, as the resource manages all of the role attachments for that IAM Policy. The best workaround for this issue is to create individual policies for each role attachment that you're wanting to attach.

reference: https://github.com/hashicorp/terraform/issues/11873#issuecomment-279418587