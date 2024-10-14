### Development Environment
[VS Code Themes](https://vscodethemes.com/)

Setting up SSM Connections in VSCode -- ~/.ssh/config on Windows:
[AWS Session Manager Page](https://docs.aws.amazon.com/systems-manager/latest/userguide/install-plugin-windows.html)
```
Host i-0############bba7
    User ssm-user
    HostName i-0############bba7
    IdentityFile C:\Users\jeremy.yelle\.ssh\i-0############bba7.key
    ProxyCommand C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "aws --profile awsProfileName --region us-east-3 ssm start-session --target %h --document-name AWS-StartSSHSession --parameters portNumber=%p"
```

### Event Driven Architecture
[AWS Event Storming](https://aws-samples.github.io/eda-on-aws/eventstorming/)

### User Authorization
[ReBAC with Neptune](https://aws.amazon.com/blogs/security/how-to-implement-relationship-based-access-control-with-amazon-verified-permissions-and-amazon-neptune/)

### Opensearch
[Opensearch Security Analytics](https://opensearch.org/docs/latest/security-analytics/)

### Terraform
[Platform Agnostic Local-Exec](https://github.com/hashicorp/terraform/issues/23785)
[Upload New File to S3 Every Time](https://stackoverflow.com/questions/76910639/how-to-update-the-source-code-as-well-with-terraform-apply)


### Windows
[Adding local drive aliases to the registry](https://superuser.com/questions/29072/how-to-make-subst-mapping-persistent-across-reboots)

### AWS Supply Chain
[Amazon Q with AWS Supply Chain](https://aws.amazon.com/about-aws/whats-new/2024/10/amazon-q-aws-supply-chain/)