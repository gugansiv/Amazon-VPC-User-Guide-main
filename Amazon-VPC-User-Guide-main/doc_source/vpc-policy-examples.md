# Amazon VPC policy examples<a name="vpc-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify VPC resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Use the Amazon VPC console](#security_iam_id-based-policy-examples-console)
+ [Create a VPC with a public subnet](#vpc-public-subnet-iam)
+ [Modify and delete VPC resources](#modify-vpc-resources-iam)
+ [Manage security groups](#vpc-security-groups-iam)
+ [Manage security group rules](#vpc-security-group-rules-iam)
+ [Launch instances into a specific subnet](#subnet-sg-example-iam)
+ [Launch instances into a specific VPC](#subnet-ami-example-iam)
+ [Additional Amazon VPC policy examples](#security-iam-additional-examples)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete Amazon VPC resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using Amazon VPC quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Use the Amazon VPC console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Amazon VPC console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the Amazon VPC resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

The following policy grants users permission to list resources in the VPC console, but not to create, update, or delete them\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeAddresses",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeClassicLinkInstances",
                "ec2:DescribeClientVpnEndpoints",
                "ec2:DescribeCustomerGateways",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeEgressOnlyInternetGateways",
                "ec2:DescribeFlowLogs",
                "ec2:DescribeInternetGateways",
                "ec2:DescribeManagedPrefixLists",
                "ec2:DescribeMovingAddresses",
                "ec2:DescribeNatGateways",
                "ec2:DescribeNetworkAcls",
                "ec2:DescribeNetworkInterfaceAttribute",
                "ec2:DescribeNetworkInterfacePermissions",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribePrefixLists",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroupReferences",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSecurityGroupRules",
                "ec2:DescribeStaleSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeTags",
                "ec2:DescribeTrafficMirrorFilters",
                "ec2:DescribeTrafficMirrorSessions",
                "ec2:DescribeTrafficMirrorTargets",
                "ec2:DescribeTransitGateways",
                "ec2:DescribeTransitGatewayVpcAttachments",
                "ec2:DescribeTransitGatewayRouteTables",
                "ec2:DescribeVpcAttribute",
                "ec2:DescribeVpcClassicLink",
                "ec2:DescribeVpcClassicLinkDnsSupport",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeVpcEndpointConnectionNotifications",
                "ec2:DescribeVpcEndpointConnections",
                "ec2:DescribeVpcEndpointServiceConfigurations",
                "ec2:DescribeVpcEndpointServicePermissions",
                "ec2:DescribeVpcEndpointServices",
                "ec2:DescribeVpcPeeringConnections",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpnConnections",
                "ec2:DescribeVpnGateways",
                "ec2:GetManagedPrefixListAssociations",
                "ec2:GetManagedPrefixListEntries"
            ],
            "Resource": "*"
        }
    ]
}
```

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, for those users, allow access only to actions that match the API operation that they need to perform\.

## Create a VPC with a public subnet<a name="vpc-public-subnet-iam"></a>

The following example enables users to create VPCs, subnets, route tables, and internet gateways\. Users can also attach an internet gateway to a VPC and create routes in route tables\. The `ec2:ModifyVpcAttribute` action enables users to enable DNS hostnames for the VPC, so that each instance launched into a VPC receives a DNS hostname\.

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": [
        "ec2:CreateVpc", 
        "ec2:CreateSubnet", 
        "ec2:DescribeAvailabilityZones",
        "ec2:CreateRouteTable", 
        "ec2:CreateRoute", 
        "ec2:CreateInternetGateway", 
        "ec2:AttachInternetGateway", 
        "ec2:AssociateRouteTable", 
        "ec2:ModifyVpcAttribute"
      ],
      "Resource": "*"
    }
   ]
}
```

The preceding policy also enables users to create a VPC using the first VPC wizard configuration option in the Amazon VPC console\. To view the VPC wizard, users must also have permission to use the `ec2:DescribeVpcEndpointServices`\. This ensures that the VPC endpoints section of the VPC wizard loads correctly\.

## Modify and delete VPC resources<a name="modify-vpc-resources-iam"></a>

You might want to control which VPC resources users can modify or delete\. For example, the following policy allows users to work with and delete route tables that have the tag `Purpose=Test`\. The policy also specifies that users can only delete internet gateways that have the tag `Purpose=Test`\. Users cannot work with route tables or internet gateways that do not have this tag\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DeleteInternetGateway",
            "Resource": "arn:aws:ec2:*:*:internet-gateway/*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Purpose": "Test"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DeleteRouteTable",
                "ec2:CreateRoute",
                "ec2:ReplaceRoute",
                "ec2:DeleteRoute"
            ],
            "Resource": "arn:aws:ec2:*:*:route-table/*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Purpose": "Test"
                }
            }
        }
    ]
}
```

## Manage security groups<a name="vpc-security-groups-iam"></a>

The following policy allows to view any security group and security group rule\. The second statement allows users to delete any security group with the tag `Stack=test` and to manage the inbound and outbound rules for any security group with the tag `Stack=test`\. The third statement requires users to tag any security groups that they create with the tag `Stack=Test`\. The fourth statement allows users to create tags when creating a security group\.

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": [
         "ec2:DescribeSecurityGroups", 
         "ec2:DescribeSecurityGroupRules", 
         "ec2:DescribeVpcs"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
         "ec2:AuthorizeSecurityGroupIngress",
         "ec2:RevokeSecurityGroupIngress",
         "ec2:UpdateSecurityGroupRuleDescriptionsIngress",
         "ec2:AuthorizeSecurityGroupEgress",
         "ec2:RevokeSecurityGroupEgress",
         "ec2:UpdateSecurityGroupRuleDescriptionsEgress",
         "ec2:ModifySecurityGroupRules",
         "ec2:DeleteSecurityGroup" 
      ],
      "Resource": "arn:aws:ec2:*:*:security-group/*",
      "Condition":{
         "StringEquals": {
            "ec2:ResourceTag/Stack": "test"
         }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
         "ec2:CreateSecurityGroup"
      ],
      "Resource": "arn:aws:ec2:*:*:security-group/*",
      "Condition": {
         "StringEquals": {
           "aws:RequestTag/Stack": "test"
         },
         "ForAllValues:StringEquals": {
           "aws:TagKeys": ["Stack"]
         }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
         "ec2:CreateTags"
      ],
      "Resource": "arn:aws:ec2:*:*:security-group/*",
      "Condition": {
        "StringEquals": {
           "ec2:CreateAction" : "CreateSecurityGroup"
        }
      }
    }
  ]
}
```

To allow users to change the security group that's associated with an instance, add the `ec2:ModifyInstanceAttribute` action to your policy\.

To allow users to change security groups for a network interface, add the `ec2:ModifyNetworkInterfaceAttribute` action to your policy\.

## Manage security group rules<a name="vpc-security-group-rules-iam"></a>

The following policy grants users permission to view all security groups and security group rules, add and remove inbound and outbound rules for the security groups for a specific VPC, and modify rule descriptions for the specified VPC\. The first statement uses the `ec2:Vpc` condition key to scope permissions to a specific VPC\. 

The second statement grants users permission to describe all security groups, security group rules, and tags\. This enables users to view security group rules in order to modify them\.

```
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect":"Allow",
    "Action": [
       "ec2:AuthorizeSecurityGroupIngress",
       "ec2:RevokeSecurityGroupIngress",
       "ec2:UpdateSecurityGroupRuleDescriptionsIngress",
       "ec2:AuthorizeSecurityGroupEgress",
       "ec2:RevokeSecurityGroupEgress",
       "ec2:UpdateSecurityGroupRuleDescriptionsEgress",
       "ec2:ModifySecurityGroupRules"
    ],
     "Resource": "arn:aws:ec2:region:account-id:security-group/*",
      "Condition": {
        "ArnEquals": {
          "ec2:Vpc": "arn:aws:ec2:region:account-id:vpc/vpc-id"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
          "ec2:DescribeSecurityGroups",
          "ec2:DescribeSecurityGroupRules",
          "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
```

## Launch instances into a specific subnet<a name="subnet-sg-example-iam"></a>

The following policy grants users permission to launch instances into a specific subnet, and to use a specific security group in the request\. The policy does this by specifying the ARN for the subnet and the ARN for the security group\. If users attempt to launch an instance into a different subnet or using a different security group, the request will fail \(unless another policy or statement grants users permission to do so\)\.

The policy also grants permission to use the network interface resource\. When launching into a subnet, the `RunInstances` request creates a primary network interface by default, so the user needs permission to create this resource when launching the instance\.

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": [
        "arn:aws:ec2:region::image/ami-*",
        "arn:aws:ec2:region:account:instance/*",
        "arn:aws:ec2:region:account:subnet/subnet-id",
        "arn:aws:ec2:region:account:network-interface/*",
        "arn:aws:ec2:region:account:volume/*",
        "arn:aws:ec2:region:account:key-pair/*",
        "arn:aws:ec2:region:account:security-group/sg-id"
      ]
    }
   ]
}
```

## Launch instances into a specific VPC<a name="subnet-ami-example-iam"></a>

The following policy grants users permission to launch instances into any subnet within a specific VPC\. The policy does this by applying a condition key \(`ec2:Vpc`\) to the subnet resource\. 

The policy also grants users permission to launch instances using only AMIs that have the tag "`department=dev`"\. 

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:region:account-id:subnet/*",
        "Condition": {
         "ArnEquals": {
            "ec2:Vpc": "arn:aws:ec2:region:account-id:vpc/vpc-id"
         }
      }
   },
   {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:region::image/ami-*",
      "Condition": {
         "StringEquals": {
            "ec2:ResourceTag/department": "dev"
            }
      }
   },
   {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": [ 
         "arn:aws:ec2:region:account:instance/*",
         "arn:aws:ec2:region:account:volume/*",
         "arn:aws:ec2:region:account:network-interface/*",
         "arn:aws:ec2:region:account:key-pair/*",
         "arn:aws:ec2:region:account:security-group/*"
         ]
      }
   ]
}
```

## Additional Amazon VPC policy examples<a name="security-iam-additional-examples"></a>

You can find additional example IAM policies related to Amazon VPC in the following documentation:
+ [ClassicLink](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html#iam-example-classiclink)
+ [Managed prefix lists](managed-prefix-lists.md#managed-prefix-lists-iam)
+ [Traffic mirroring](https://docs.aws.amazon.com/vpc/latest/mirroring/traffic-mirroring-security.html)
+ [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-authentication-access-control.html#tgw-example-iam-policies)
+ [VPC endpoints and VPC endpoint services](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-iam.html)
+ [VPC endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html)
+ [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/security-iam.html)
+ [AWS Wavelength](https://docs.aws.amazon.com/wavelength/latest/developerguide/wavelength-policy-examples.html)