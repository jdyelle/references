## Development Environment
Setting up SSM Connections in VSCode -- ~/.ssh/config on Windows:
```
Host i-0############bba7
    User ssm-user
    HostName i-0############bba7
    IdentityFile C:\Users\jeremy.yelle\.ssh\i-0############bba7.key
    ProxyCommand C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "aws --profile awsProfileName --region us-east-3 ssm start-session --target %h --document-name AWS-StartSSHSession --parameters portNumber=%p"
```

## Event Driven Architecture
[AWS Event Storming](https://aws-samples.github.io/eda-on-aws/eventstorming/)


## Opensearch
[Opensearch Security Analytics](https://opensearch.org/docs/latest/security-analytics/)