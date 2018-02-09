## Export AWS CLI profile to Environment variable

```bash
export AWS_ACCESS_KEY_ID=$(aws configure get default.aws_access_key_id)
```