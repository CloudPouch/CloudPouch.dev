# CloudPouch
[Website](https://cloudpouch.dev) • [Releases](https://github.com/CloudPouch/CloudPouch.dev/releases) • [Buy license](https://cloudpouch.dev/#pricing)

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
                "ce:GetCostAndUsage",
                "organizations:ListAccounts",
                "ec2:Describe*",
                "dynamodb:DescribeTable",
                "dynamodb:ListTables",
                "cloudwatch:GetMetricStatistics",
                "application-autoscaling:DescribeScalableTargets"
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
Insights check your resources in the AWS cloud and provide useful information for cost optimization. They can detect *waste*, for example unattached EBS drives or wrongly configured resources such us over-provisioned DynamoDB tables.

* EC2 - Other insights use following privileges:
    * `ec2:Describe*`
* DynamoDB insights use:
    * `dynamodb:DescribeTable`
    * `dynamodb:ListTables`
    * `cloudwatch:GetMetricStatistics`
    * `application-autoscaling:DescribeScalableTargets`



