# Terraform Module - Watchmaker Windows AutoScaling Group

This module utilizes the Terraform CloudFormation resource in conjunction
with an AWS CloudFormation template to deploy a Watchmaker Windows AutoScaling Group.

<!-- BEGIN TFDOCS -->
## Providers

| Name | Version |
|------|---------|
| aws | n/a |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:-----:|
| AmiId | (Required) ID of the AMI to launch | `string` | n/a | yes |
| AppScriptUrl | (Optional) S3 URL to the .ps1 or .bat application script in an S3 bucket (s3://). Leave blank to launch without an application script. If specified, an appropriate InstanceRole is required | `string` | n/a | yes |
| AppVolumeDevice | #(Optional) Device to mount an extra EBS volume. Leave blank to launch without an extra application volume | `string` | n/a | yes |
| AppVolumeSnapshotId | (Optional) EBS Snapshot ID from which to create the AppVolume. "AppVolumeSize" must be equal or greater than the size of the snapshot. Ignored if "AppVolumeDevice" is blank | `string` | n/a | yes |
| AsgSnsArn | (Optional) The Amazon Resource Name (ARN) of the Amazon Simple Notification Service (SNS) topic to send ASG events to. NOTE: Must be defined in conjunction with ASGNotificationTypes. | `string` | n/a | yes |
| CloudWatchAgentUrl | (Optional) S3 URL to CloudWatch Agent installer. Example: s3://amazoncloudwatch-agent/windows/amd64/latest/amazon-cloudwatch-agent.msi | `string` | n/a | yes |
| IamRoleArn | (Optional) The ARN of an IAM role that AWS CloudFormation assumes to create the stack. If you don't specify a value, AWS CloudFormation uses the role that was previously associated with the stack. If no role is available, AWS CloudFormation uses a temporary session that is generated from your user credentials | `string` | n/a | yes |
| InstanceRole | (Optional) IAM instance role to apply to the instance | `string` | n/a | yes |
| KeyPairName | (Required) Public/private key pairs allow you to securely connect to your instance after it launches | `string` | n/a | yes |
| LoadBalancerNames | (Optional) Comma-separated string of Classic ELB Names to associate with the Autoscaling Group; conflicts with TargetGroupArns | `string` | n/a | yes |
| Name | (Required) Name of CloudFormation Stack | `string` | n/a | yes |
| PatchGroup | (Optional) Key value of the Patch Group tag. Controls whether to create a PatchGroup tag that can be leveraged via SSM to auto-update instances. | `string` | n/a | yes |
| PolicyBody | (Optional) String containing the stack policy body. Conflicts with PolicyUrl | `string` | n/a | yes |
| PolicyUrl | (Optional) URL to a file containing the stack policy. Conflicts with PolicyBody | `string` | n/a | yes |
| ScaleDownSchedule | (Optional) Scheduled Action in cron-format (UTC) to scale down to MinCapacity; ignored if empty or ScaleUpSchedule is unset (E.g. "0 0 * * *") | `string` | n/a | yes |
| ScaleUpSchedule | (Optional) Scheduled Action in cron-format (UTC) to scale up to MaxCapacity; ignored if empty or ScaleDownSchedule is unset (E.g. "0 10 * * Mon-Fri") | `string` | n/a | yes |
| SecurityGroupIds | (Required) List of security groups to apply to the instance | `string` | n/a | yes |
| SubnetIds | (Required) List of subnets to associate to the Autoscaling Group | `string` | n/a | yes |
| TargetGroupArns | (Optional) Comma-separated string of Target Group ARNs to associate with the Autoscaling Group; conflicts with LoadBalancerNames | `string` | n/a | yes |
| WatchmakerAdminGroups | (Optional) Colon-separated list of domain groups that should have admin permissions on the EC2 instance | `string` | n/a | yes |
| WatchmakerAdminUsers | (Optional) Colon-separated list of domain users that should have admin permissions on the EC2 instance | `string` | n/a | yes |
| WatchmakerConfig | (Optional) URL to a Watchmaker config file | `string` | n/a | yes |
| WatchmakerEnvironment | (Optional) Environment in which the instance is being deployed | `string` | n/a | yes |
| WatchmakerExtraArgs | (Optional) Additional parameters to be passed to the Watchmaker CLI | `string` | n/a | yes |
| WatchmakerOuPath | (Optional) DN of the OU to place the instance when joining a domain. If blank and WatchmakerEnvironment enforces a domain join, the instance will be placed in a default container. Leave blank if not joining a domain, or if WatchmakerEnvironment is false | `string` | n/a | yes |
| WatchmakerStandaloneUrl | (Optional) HTTP/S URL to a Watchmaker standalone executable. The file will be retrieved and used to run Watchmaker, instead of installing Watchmaker from PyPi | `string` | n/a | yes |
| WatchmakerVersion | (Optional) Watchmaker version to install. When blank (the default) the latest version will be installed. Used only when Watchmaker is installed from PyPi | `string` | n/a | yes |
| AppScriptParams | (Optional) Parameter string to pass to the application script. Ignored if AppScriptUrl is blank | `string` | `""` | no |
| AppVolumeSize | (Optional) Size in GB of the EBS volume to create. Ignored if AppVolumeDevice is blank | `string` | `"1"` | no |
| AppVolumeType | (Optional) Type of EBS volume to create. Ignored if AppVolumeDevice is blank | `string` | `"gp2"` | no |
| AsgMetrics | (Optional) The list of ASG metrics to collect. Must define EnableASGMetricsCollection to enable. Define MetricsCollectionGranularity and leave this option blank to collect all metrics | `list(string)` | `[]` | no |
| AsgNotificationTypes | (Optional) A list of event types that trigger a notification. Event types can include any of the following types: autoscaling:EC2\_INSTANCE\_LAUNCH, autoscaling:EC2\_INSTANCE\_LAUNCH\_ERROR, autoscaling:EC2\_INSTANCE\_TERMINATE, autoscaling:EC2\_INSTANCE\_TERMINATE\_ERROR, and autoscaling:TEST\_NOTIFICATION. NOTE: Must be defined in conjunction with ASGSNSARN. | `list(string)` | `[]` | no |
| Capabilities | (Optional) A list of capabilities. Valid values: CAPABILITY\_IAM or CAPABILITY\_NAMED\_IAM | `list(string)` | `[]` | no |
| CfnEndpointUrl | (Optional) URL to the CloudFormation Endpoint. e.g. https://cloudformation.us-east-1.amazonaws.com | `string` | `"https://cloudformation.us-east-1.amazonaws.com"` | no |
| CloudWatchAppLogs | (Optional) List of application log file paths to send to CloudWatch. Example: C:\dir1\file1,C:\dir2\file2,C:\dir3\file3 | `list(string)` | `[]` | no |
| DesiredCapacity | (Optional) Desired number of instances in the Autoscaling Group | `string` | `"1"` | no |
| DisableRollback | (Optional) Set to true to disable rollback of the stack if stack creation failed. Conflicts with OnFailure | `string` | `false` | no |
| EbsOptimized | (Optional) Specifies whether the launch configuration is optimized for EBS I/O. This optimization provides dedicated throughput to Amazon EBS and an optimized configuration stack to provide optimal EBS I/O performance. Warning: Stack creation will fail if set to true and the instance type does not support EBS Optimization. See complete list of supported instances here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html#ebs-optimization-support | `bool` | `false` | no |
| InstanceTerminationPolicies | (Optional) A list of policies that are used to select the instances to terminate. The policies are executed in the order that you list them. | `list(string)` | `[]` | no |
| InstanceType | (Optional) Amazon EC2 instance type | `string` | `"t2.micro"` | no |
| MaxCapacity | (Optional) Maximum number of instances in the Autoscaling Group | `string` | `"2"` | no |
| MinCapacity | (Optional) Minimum number of instances in the Autoscaling Group | `string` | `"1"` | no |
| NoPublicIp | (Optional) Controls whether to assign the instance a public IP. Recommended to leave at true _unless_ launching in a public subnet | `bool` | `true` | no |
| NoReboot | (Optional) Controls whether to reboot the instance as the last step of cfn-init execution | `bool` | `false` | no |
| NotificationArns | (Optional) A list of SNS topic ARNs to publish stack related events | `list(string)` | `[]` | no |
| OnFailureAction | (Optional) Action to be taken if stack creation fails. This must be one of: DO\_NOTHING, ROLLBACK, or DELETE. Conflicts with DisableRollback | `string` | `"DO_NOTHING"` | no |
| PypiIndexUrl | (Optional) URL to the PyPi Index | `string` | `"https://pypi.org/simple"` | no |
| PythonInstaller | (Optional) URL to the Python Installer Executable | `string` | `"https://www.python.org/ftp/python/3.6.4/python-3.6.4-amd64.exe"` | no |
| RootVolumeSize | (Optional) Root Volume Size in GB **NOTE** This value can be set larger than the default (30GB) but NOT smaller. | `string` | `"30"` | no |
| StackTags | (Optional) A map of tag keys/values to associate with this stack | `map(string)` | `{}` | no |
| TimeoutInMinutes | (Optional) The amount of time that can pass before the stack status becomes CREATE\_FAILED | `string` | `"30"` | no |
| ToggleCfnInitUpdate | (Optional) A/B toggle that forces a change to instance metadata, triggering the cfn-init update sequence | `string` | `"A"` | no |
| ToggleNewInstances | (Optional) A/B toggle that forces a change to instance userdata, triggering new instances via the Autoscale update policy | `string` | `"A"` | no |
| WatchmakerBootstrapper | (Optional) URL to the Watchmaker PowerShell bootstrapper for Windows | `string` | `"https://raw.githubusercontent.com/plus3it/watchmaker/master/docs/files/bootstrap/watchmaker-bootstrap.ps1"` | no |

## Outputs

| Name | Description |
|------|-------------|
| watchmaker-win-autoscale | CloudFormation stack object for watchmaker-win-autoscale |

<!-- END TFDOCS -->
