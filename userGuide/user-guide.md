# CloudPouch User Guide

## Configuration files
The `config.json` file location depends on the OS you're using:

* MacOs - `/Users/<YOUR_USER_NAME>/Library/Application Support/CloudPouch/config.json`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\config.json`
* Linux - `~/.config/CloudPouch/config.json`
## Logs
The `main.log` file location depends on the OS you're using:

* MacOs - `/Users/<YOUR_USER_NAME>/Library/Logs/CloudPouch`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\logs`
* Linux - `~/.config/CloudPouch/`

## Cached data
CloudPouch stores cached data as JSON files on your local hard drive:
* MacOs - `/Users/<YOUR_USER_NAME>/Library/Application Support/CloudPouch/data`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\data`
* Linux - ``


## Minimal IAM User privileges
If you want to use a dedicated IAM user with minimal privileges please use the following policy:
```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CloudPouchMinimalAccess",
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:DescribeScalableTargets",
                "ce:GetCostAndUsage",
                "cloudwatch:GetMetricStatistics",
                "dynamodb:DescribeTable",
                "dynamodb:ListTables",
                "ec2:Describe*",
                "ebs:ListSnapshotBlocks",
                "ebs:ListChangedBlocks",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeTargetHealth",
                "logs:DescribeLogGroups",
                "organizations:ListAccounts",
                "rds:DescribeDBClusters",
                "rds:DescribeDBInstances",
                "rds:DescribeDBSnapshots"
            ],
            "Resource": "*"
        }
    ]
}
```
Last change for version [1.24.0](https://github.com/CloudPouch/CloudPouch.dev/releases/tag/v1.24.0).

## Optional: Configuring Certificates in CloudPouch Application
The CloudPouch includes an option that enables users to configure and use a certificate when connecting to the internet. This feature is particularly beneficial for users operating within corporate networks that frequently have stringent security protocols, often mandating certificate use for internet resource access.

The CloudPouch supports the use of custom and global SSL/TLS certificates in PEM format (`.pem`). These could be certificates issued by widely recognized CAs, or those signed by private or self-signed authorities. Please make sure to specify the correct path to your certificate when configuring your CloudPouch application.

To set up the certificate, please define the path to the certificate file in the `config.json` file, as shown below:
```JSON
{
  "certificatePath": "<Path to your .pem certificate file>"
}
```

Here, `certificatePath` should contain the full path to your `.pem` file, including the file name. Please ensure you have the necessary read permissions to access this file.

Ensure to restart the CloudPouch application for the new certificate settings to take effect.

Should you need more detailed information about supported certificates, refer to the public documentation regarding `AWS_CA_BUNDLE` and `NODE_EXTRA_CA_CERTS` on the Internet.

### File location
The `config.json` file location depends on the OS you're using:

* MacOs - `/Users/<YOUR_USER_NAME>/Library/Application Support/CloudPouch/config.json`
* Windows - `c:\Users\<YOUR_USER_NAME>\AppData\Roaming\CloudPouch\config.json`
* Linux - `~/.config/CloudPouch/config.json`


Certificate support was introduces in version [1.25.0](https://github.com/CloudPouch/CloudPouch.dev/releases/tag/v1.25.0).

## Create CloudFormation stack
Click this button to create `CloudPouch-access-policy-stack` on your AWS account with the IAM policy that you can attach to any IAM Role or IAM User.

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=CloudPouch-access-policy-stack&templateURL=https://cloudpouch-public.s3.amazonaws.com/cloudformation-policy.yml)

<details>
<summary><b>Step-by-step guide of CloudFormation deployment</b></summary><p>

1. This is the first CloudFormation service console with the template already pre-loaded. Click `Next`.
![](../images/cf-creation-01.png)
1. There are parameters to set. Click `Next`.
![](../images/cf-creation-02.png)
1. Leaver everything as is. Click `Next`.
![](../images/cf-creation-03.png)
![](../images/cf-creation-04.png)
1. Review and click `Next`.
![](../images/cf-creation-05.png)
![](../images/cf-creation-06.png)
1. CloudFormation deployment starts.
![](../images/cf-creation-07.png)
1. After a moment it is finished.
![](../images/cf-creation-08.png)
1. Go to [IAM Policies](https://us-east-1.console.aws.amazon.com/iamv2/home#/policies) tab and find the policy named `CloudPouch-costs-policy`.
![](../images/cf-creation-09.png)
1. Now you need to add this policy to an IAM User or a Role.
</p>
</details>

<br />

### Attach policy to an IAM User
1. After Policy is created go to the [IAM Users tab](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/users), select a user and click `Add Permissions` button (select again `Add Permissions` from the dropdown list).
1. Select `Attach policies directly` and in the search below enter the name of the newly created IAM Policy: `CloudPouch-costs-policy` ![Attach policies directly](../images/add_permissions.png)
1. Tick checkbox next the it and click `Next`
1. On the next screen click `Add Permissions` button.
1. Done ✅
### Policy explanation
#### Necessary privileges
* `ce:GetCostAndUsage` is crucial as allows to fetch cost data. 
* `organizations:ListAccounts` used to resolve names of your accounts in the AWS Organizations. Used only when you have paying account.

#### Insights privileges 
Insights check your resources in the AWS cloud and provide useful information for cost optimization. They can detect *waste*, for example unattached EBS drives or wrongly configured resources such as over-provisioned DynamoDB tables.

* EC2 - Other insights use following privileges:
    * `ec2:Describe*`
* DynamoDB insights use:
    * `dynamodb:DescribeTable`
    * `dynamodb:ListTables`
    * `cloudwatch:GetMetricStatistics`
    * `application-autoscaling:DescribeScalableTargets`
* CloudWatch insights use:
    * `logs:DescribeLogGroups`
* RDS insights use:
    - `rds:DescribeDBClusters`
    - `rds:DescribeDBInstances`
    - `rds:DescribeDBSnapshots`
* ELB insights use:
    - `elasticloadbalancing:DescribeLoadBalancers`
    - `elasticloadbalancing:DescribeTargetGroups`
    - `elasticloadbalancing:DescribeTargetHealth`


# Troubleshooting

## Ubuntu Linux problem

When application doesn't want to start on Linux, check logs for `GPU process isn't usable. Goodbye.` message. Something similar to:

```
12:48:08.895 › Checking for update
12:48:08.931 › Generated new staging user ID: 08751b29-ea12-5a1c-b8b6-4256748d7cd6
(node:708150) ExtensionLoadWarning: Warnings loading extension at /home/<USER_NAME>/.config/CloudPouch/extensions/lmhkpmbekcpmknklioeibfkpmmfibljd:
  Unrecognized manifest key 'commands'.
  Unrecognized manifest key 'homepage_url'.
  Unrecognized manifest key 'page_action'.
  Unrecognized manifest key 'short_name'.
  Unrecognized manifest key 'update_url'.
  Permission 'notifications' is unknown or URL pattern is malformed.
  Permission 'contextMenus' is unknown or URL pattern is malformed.
  Cannot load extension with file or directory name _metadata. Filenames starting with "_" are reserved for use by the system.

(Use `cloudpouch --trace-warnings ...` to show where the warning was created)
Added Extension:  Redux DevTools
12:48:10.316 › Update for version 1.19.0 is not available (latest version: 1.19.0, downgrade is disallowed).
12:48:10.317 › checkForUpdatesAndNotify called, downloadPromise is null
[708150:0727/124810.448196:FATAL:gpu_data_manager_impl_private.cc(415)] GPU process isn't usable. Goodbye.
Trace/breakpoint trap (core dumped)
```
### solution
Run application with `--in-process-gpu` parameter. For example:
```
./Downloads/CloudPouch-1.19.0.AppImage --in-process-gpu
```
