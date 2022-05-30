# CloudPouch
[Website](https://cloudpouch.dev) • [Releases](https://github.com/CloudPouch/CloudPouch.dev/releases) • [Buy license](https://cloudpouch.dev/#pricing) • [User Guide](https://github.com/CloudPouch/CloudPouch.dev/blob/main/userGuide/user-guide.md)

## Minimal IAM User privledges
If you want to use a dedicated IAM user with minial privledges please use the following policy:
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
                "logs:DescribeLogGroups",
                "organizations:ListAccounts"
            ],
            "Resource": "*"
        }
    ]
}
```
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


## AWS SSO Configuration
To use AWS SSO you need to properly configure your SSO profile (in `~/.aws/config` file), according to the AWS documentation [Configuring the AWS CLI to use AWS Single Sign-On](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html).
