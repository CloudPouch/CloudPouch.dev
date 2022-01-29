# CloudPouch
[Website](https://cloudpouch.dev) • [Releases](https://github.com/CloudPouch/CloudPouch.dev/releases) • [Buy license](https://cloudpouch.dev/#pricing)

# Minimal IAM User privledges
IF you want to use a dedicated IAM user with minial privledges please use the following policy:
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
                "cloudwatch:GetMetricStatistics"
            ],
            "Resource": "*"
        }
    ]
}
```
