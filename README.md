[comment]: # "Auto-generated SOAR connector documentation"
# AWS CloudTrail

Publisher: Splunk  
Connector Version: 2.2.7  
Product Vendor: AWS  
Product Name: CloudTrail  
Product Version Supported (regex): ".\*"  
Minimum Product Version: 4.9.39220  

This app integrates with AWS CloudTrail to perform various investigative actions

[comment]: # " File: README.md"
[comment]: # "  Copyright (c) 2018-2021 Splunk Inc."
[comment]: # ""
[comment]: # "  SPLUNK CONFIDENTIAL - Use or disclosure of this material in whole or in part"
[comment]: # "  without a valid written license from Splunk Inc. is PROHIBITED."
[comment]: # ""
## Asset Configuration

There are two ways to configure an AWS CloudTrail asset. The first is to configure the
**access_key** , **secret_key** and **region** variables. If it is preferred to use a role and
Phantom is running as an EC2 instance, the **use_role** checkbox can be checked instead. This will
allow the role that is attached to the instance to be used. Please see the [AWS EC2 and IAM
documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
for more information.

## Assumed Role Credentials

The optional **credentials** action parameter consists of temporary **assumed role** credentials
that will be used to perform the action instead of those that are configured in the **asset** . The
parameter is not designed to be configured manually, but should instead be used in conjunction with
the Phantom AWS Security Token Service app. The output of the **assume_role** action of the STS app
with data path **assume_role\_\<number>:action_result.data.\*.Credentials** consists of a dictionary
containing the **AccessKeyId** , **SecretAccessKey** , **SessionToken** and **Expiration** key/value
pairs. This dictionary can be passed directly into the credentials parameter in any of the following
actions within a playbook. For more information, please see the [AWS Identity and Access Management
documentation](https://docs.aws.amazon.com/iam/index.html) .


### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a CloudTrail asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**Access Key** |  optional  | password | Access Key
**Secret Key** |  optional  | password | Secret Key
**Region** |  required  | string | Default Region
**use_role** |  optional  | boolean | Use attached role when running Phantom in EC2

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using the supplied configuration  
[describe trails](#action-describe-trails) - Retrieve settings for trails associated with the current region and the multi-region trails  
[run query](#action-run-query) - Lookup the management events captured by CloudTrail  

## action: 'test connectivity'
Validate the asset configuration for connectivity using the supplied configuration

Type: **test**  
Read only: **True**

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'describe trails'
Retrieve settings for trails associated with the current region and the multi-region trails

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**include_shadow_trails** |  optional  | Inform command to include shadow trails | boolean | 
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.status | string |  |   success  failed 
action_result.parameter.include_shadow_trails | boolean |  |   True  False 
action_result.data.\*.HasCustomEventSelectors | boolean |  |   True  False 
action_result.data.\*.HomeRegion | string |  |   us-east-1 
action_result.data.\*.IncludeGlobalServiceEvents | boolean |  |   True  False 
action_result.data.\*.IsMultiRegionTrail | boolean |  |   True  False 
action_result.data.\*.IsOrganizationTrail | boolean |  |   True  False 
action_result.data.\*.LogFileValidationEnabled | boolean |  |   True  False 
action_result.data.\*.Name | string |  |   test-cloudtrail 
action_result.data.\*.S3BucketName | string |  |   test-bucket 
action_result.data.\*.SnsTopicARN | string |  `aws arn`  |   arn:aws:sns:us-west-2:123456789012:test-splunk-aws-addon-sns-notifications 
action_result.data.\*.SnsTopicName | string |  `aws arn`  |   arn:aws:sns:us-west-2:123456789012:test-addon-sns 
action_result.data.\*.TrailARN | string |  `aws arn`  |   arn:aws:cloudtrail:us-east-1:123456789012:trail/test-cloudtrail 
action_result.summary.message | string |  |   Received 3 trails 
action_result.message | string |  |   Message: Received 3 trails 
summary.total_objects | numeric |  |   1 
summary.total_objects_successful | numeric |  |   1 
action_result.parameter.credentials | string |  `aws credentials`  |   {'AccessKeyId': 'ASIASJL6ZZZZZ3M7QC2J', 'Expiration': '2021-06-07 22:28:04', 'SecretAccessKey': 'ZZZZZAmvLPictcVBPvjJx0d7MRezOuxiLCMZZZZZ', 'SessionToken': 'ZZZZZXIvYXdzEN///////////wEaDFRU0s4AVrw0k0oYICK4ATAzOqzAkg9bHY29lYmP59UvVOHjLufOy4s7SnAzOxGqGIXnukLis4TWNhrJl5R5nYyimrm6K/9d0Cw2SW9gO0ZRjEJHWJ+yY5Qk2QpWctS2BGn4n+G8cD6zEweCCMj+ScI5p8n7YI4wOdvXvOsVMmjV6F09Ujqr1w+NwoKXlglznXGs/7Q1kNZOMiioEhGUyoiHbQb37GCKslDK+oqe0KNaUKQ96YCepaLgMbMquDgdAM8I0TTxUO0o5ILF/gUyLT04R7QlOfktkdh6Qt0atTS+xeKi1hirKRizpJ8jjnxGQIikPRToL2v3ZZZZZZ=='}   

## action: 'run query'
Lookup the management events captured by CloudTrail

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**attribute_key** |  optional  | Select an Attribute to query by (or leave blank to retrieve all records) | string | 
**attribute_value** |  optional  | Specify the Value by which to search. Note that true/false values must be lower-case | string | 
**start_date** |  optional  | Start date in the format of yyyy-mm-dd (e.g. 2019-12-25) | string | 
**end_date** |  optional  | End date in the format of yyyy-mm-dd (e.g. 2019-12-25) | string | 
**max_results** |  optional  | Max results to return | numeric | 
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.status | string |  |   success  failed 
action_result.parameter.attribute_key | string |  |   EventName  Username 
action_result.parameter.attribute_value | string |  |   DescribeTable  false  true 
action_result.parameter.end_date | string |  |   2021-02-10  1980-08-12 
action_result.parameter.max_results | numeric |  |   123 
action_result.parameter.start_date | string |  |   2021-02-10  1980-08-12 
action_result.data.\*.AccessKeyId | string |  |   ABCDEFGHI1234567890 
action_result.data.\*.EventId | string |  |   1234abcd-12ab-ab12-ab12-123456abcdef 
action_result.data.\*.EventName | string |  |   testEvents 
action_result.data.\*.EventSource | string |  |   cloudtrail.amazonaws.com 
action_result.data.\*.EventTime | string |  |   2019-07-30 10:48:04 
action_result.data.\*.ExtractedCloudTrailEvent.apiVersion | string |  |   2015-02-01 
action_result.data.\*.ExtractedCloudTrailEvent.awsRegion | string |  |   us-east-1 
action_result.data.\*.ExtractedCloudTrailEvent.errorCode | string |  |   AccessDenied 
action_result.data.\*.ExtractedCloudTrailEvent.errorMessage | string |  |   User: arn:aws:sts::123456789012:assumed-role/test-role/parseRegistrationEmailBase64 is not authorized to perform: logs:CreateLogStream on resource: arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/parseRegistrationEmailBase64:log-stream:2019/07/30/[$LATEST]1234abcd12abab12ab12123456abcdef 
action_result.data.\*.ExtractedCloudTrailEvent.eventID | string |  |   1234abcd-12ab-ab12-ab12-123456abcdef 
action_result.data.\*.ExtractedCloudTrailEvent.eventName | string |  |   testEvents 
action_result.data.\*.ExtractedCloudTrailEvent.eventSource | string |  |   cloudtrail.amazonaws.com 
action_result.data.\*.ExtractedCloudTrailEvent.eventTime | string |  |   2019-07-30T10:48:04Z 
action_result.data.\*.ExtractedCloudTrailEvent.eventType | string |  |   AwsApiCall 
action_result.data.\*.ExtractedCloudTrailEvent.eventVersion | string |  |   1.05 
action_result.data.\*.ExtractedCloudTrailEvent.readOnly | boolean |  |   True  False 
action_result.data.\*.ExtractedCloudTrailEvent.recipientAccountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.requestID | string |  |   1234abcd-12ab-ab12-ab12-123456abcdef 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters | string |  |  
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.Filters.UpdatedAt.\*.End | string |  |   2019-07-30T10:47:25.908919Z 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.Filters.UpdatedAt.\*.Start | string |  |   2019-07-30T10:46:25.888382Z 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.MaxResults | numeric |  |   100 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.accountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.agentName | string |  |   test-ssm-agent 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.agentStatus | string |  |   Active 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.agentVersion | string |  |   2.3.542.0 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.computerName | string |  |   ip-123-12-12-123.ec2.internal 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.defaultOnly | boolean |  |   True  False 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.encryptionContext.aws:lambda:FunctionArn | string |  `aws arn`  |   arn:aws:lambda:us-east-1:123456789012:function:test--LambdaAssets-ABCDE1234567 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.endTime | string |  |   Jul 30, 2019 10:32:45 AM 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.environmentId | string |  |   e-abcde12345 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.externalId | string |  |   abcdEFGhi 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.fileSystemId | string |  |   fs-1234abcd 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.iPAddress | string |  `ip`  |   122.122.122.122 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.instanceId | string |  |   i-abcdefg123456 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.limit | string |  |   1000 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.lookupAttributes.\*.attributeKey | string |  |   ReadOnly 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.lookupAttributes.\*.attributeValue | string |  |   false 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.marker | string |  |   lslTXFcbLQKkbaabbccddh8Iuhhi7cd/c3sgqk3eiDRabcdfergtskS7Bo66iQPTMpVOHZkGsDz1LI7e+TWIc3wm1hIW/abcdefg12345/jpTjHbLNqsrBsgR/ee2eXoJp0e6ORRUuOVOaR5nxZmGTGQADFACSslcabxXl3/jDI4rfFnIsUVdzTLBgPF1hzwrE1f3lcdkBvUp+QgY+Pn3w5Qvo3bzKGVTASYpusBX+bSTmSHOiGxLITL+mvpL/sXfkK+8MMwtRyKS4b64do4cE7mPFzUUluuj8W69Cm74OK1NgiKGIYu3Bw7lSNrLd+q3+wBGNLTnq7RWa21Xjxe5me9SyEscOWAwjnLEf9QpeMhWcWzbQhxtGas3WJ8MmCebU1auV7TUkBvPVomaBnFP+5l4rh7OjJMy7bcV+dw0TAkgzMHloQ5dXhvm751bUYR7A85kKM4EcBK3HokqBzefLfgRU99q0jOYJVftlPhUGWp+T8EoO4fBXNHqRXmq4aLpzPpQMzMc5vGlkrP7x7GF9emA= 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.maxItems | numeric |  |   100 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.maxRecords | numeric |  |   1000 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.maxResults | numeric |  |   1000 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.mountTargetId | string |  |   fsmt-1234abcd 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.networkInterfaceId | string |  |   eni-abcdefg1234567 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.platformName | string |  |   Amazon Linux 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.platformType | string |  |   Linux 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.platformVersion | string |  |   2 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.policyArn | string |  `aws arn`  |   arn:aws:iam::123456789012:policy/testPolicy 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.requestContext.awsAccountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.resource | string |  `aws arn`  |   arn:aws:lambda:us-east-1:123456789012:function:test 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.resourceArn | string |  `aws arn`  |   arn:aws:elasticbeanstalk:us-east-1:123456789012:environment/test-target/testtarget-env 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.resourceId | string |  |   ws-abcdef1234 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.roleArn | string |  `aws arn`  |   arn:aws:iam::123456789012:role/test-role 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.roleName | string |  |   test-role 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.roleSessionName | string |  |   test 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.stackName | string |  |   awseb-e-1234abcde-stack 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.startTime | string |  |   Jul 30, 2019 10:24:27 AM 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.topicArn | string |  `aws arn`  |   arn:aws:sns:us-east-1:123456789012:test-code-SNSTopic-ABCDEFGH123 
action_result.data.\*.ExtractedCloudTrailEvent.requestParameters.versionId | string |  |   v1 
action_result.data.\*.ExtractedCloudTrailEvent.resources.\*.ARN | string |  `aws arn`  |   arn:aws:iam::123456789012:role/test-role 
action_result.data.\*.ExtractedCloudTrailEvent.resources.\*.accountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.resources.\*.type | string |  |   AWS::IAM::Role 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements | string |  |  
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.Message | string |  |   User: arn:aws:sts::123456789012:assumed-role/test-role/test is not authorized to perform: eks:ListClusters on resource: arn:aws:eks:us-east-1:123456789012:cluster/\* 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.assumedRoleUser.arn | string |  `aws arn`  |   arn:aws:sts::123456789012:assumed-role/test-role/test 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.assumedRoleUser.assumedRoleId | string |  |   ABCDEFGHI123456789:test 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.credentials.accessKeyId | string |  |   ABCDEFGHI123456789 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.credentials.expiration | string |  |   Jul 30, 2019 11:47:45 AM 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.credentials.sessionToken | string |  |   AgoJb3JpZ2luX2Vabcdefghi123456789EQCIGmmjSmuKp8Dk8AmEx1yuMYYFSpD7h8tuqJDX0BdawM1AiBOV74zA4lnwD6uo5aj50a6pTmrDDFxsI9Gz7B+wg+96iqXAgiM//////////abcdefghijklmn1234567890/uI5w04fo9r5LKusB05HceTN5TJRBkLrVOK+yGMXURedACeF1kXJ0rID3FC18eDZg16wFAVWVy4cQ/l77qDKR7HIjFrhaCGMzWWIDZXdv8uDjgq+TDfWOuS02BnnSq6sZ89PFLn32tXnshrEHKomzqqhW+7ObRY+z3OxoMIWw4vCIkJauWmygzXYTn4Evskv8lpf4sTQ6q2FySLQm+U8afbVSJ4wyztmYphwcfHDHNRTz04Q8hr6L4Fk6+xg+gqC9h5cNKqrUV+PW9SZQ+yePFRe7w5viJimsl9C5A6Av5YUMul0gfVtd8/s6JudVLf/GQ0EI72pIDTDRwIDqBTq1AXJX4J9YqaODMiUjBePvsGZdwpvUh0jK/QuSJtj2314Mieo61MxDBzCRnkpGglhRoGyQsBsU39GVowbmyGuh9Ptn5rd5/qWXnjKTfWUXsxoxl2r7tc/j8gIbcmiHFtAM05KUaehB4d5u1m+Yz1jxItWpEi9rBxs4OFxQqPoXrLwGKY8nqunAsTjKF/bgMoj7PBeaLq2b8yhaAiTUUUDrb70Isl0RGE7EQSdSn7fu33OgK+j/Ak8= 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.alias | string |  |   d-12345abcdefg 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.customerUserName | string |  `user name`  |   testUser 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.directoryId | string |  |   d-12345abcdefg 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.directoryName | string |  |   corp.amazonworkspaces.com 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.directoryType | string |  |   SIMPLE_AD 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.dnsIpAddresses | string |  `ip`  |   123.12.1.123 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.iamRoleId | string |  `aws arn`  |   arn:aws:iam::123456789012:role/test-role 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.registrationCode | string |  |   ABC123+EFG456 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.state | string |  |   REGISTERED 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.subnetIds | string |  |   subnet-1234567890abcdefge 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.workspaceCreationProperties.enableInternetAccess | boolean |  |   True  False 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.workspaceCreationProperties.enableWorkDocs | boolean |  |   True  False 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.workspaceCreationProperties.userEnabledAsLocalAdministrator | boolean |  |   True  False 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.directories.\*.workspaceSecurityGroupId | string |  |   sg-1234abcde1234 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.tags.aws:cloudformation:logical-id | string |  |   LambdaAssets 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.tags.aws:cloudformation:stack-id | string |  `aws arn`  |   arn:aws:cloudformation:us-east-1:123456789012:stack/test/1234abcd-12ab-ab12-ab12-123456abcdef 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.tags.aws:cloudformation:stack-name | string |  |   test 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.bundleId | string |  |   wsb-abc123abc 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.computerName | string |  |   IP-ABCD123AB 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.directoryId | string |  |   d-12345abcdefg 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.ipAddress | string |  `ip`  |   123.12.1.123 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.state | string |  |   STOPPED 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.subnetId | string |  |   subnet-1234567890abcdefge 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.userName | string |  `user name`  |   testUser 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceId | string |  |   ws-abcde12345 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceProperties.computeTypeName | string |  |   STANDARD 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceProperties.rootVolumeSizeGib | numeric |  |   80 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceProperties.runningMode | string |  |   AUTO_STOP 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceProperties.runningModeAutoStopTimeoutInMinutes | numeric |  |   60 
action_result.data.\*.ExtractedCloudTrailEvent.responseElements.workspaces.\*.workspaceProperties.userVolumeSizeGib | numeric |  |   50 
action_result.data.\*.ExtractedCloudTrailEvent.sharedEventID | string |  |   1234abcd-12ab-ab12-ab12-123456abcdef 
action_result.data.\*.ExtractedCloudTrailEvent.sourceIPAddress | string |  `ip`  |   12.123.12.123 
action_result.data.\*.ExtractedCloudTrailEvent.userAgent | string |  |   aws-sdk-java/1.11.569 Linux/4.14.128-87.105.amzn1.x86_64 test_HotSpot(TM)_64-Bit_Server_VM/25.202-b08 test/1.8.0_202 test1/2.4.15 vendor/test_Corporation 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.accessKeyId | string |  |   ABCDEFGHI123456789 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.accountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.arn | string |  `aws arn`  |   arn:aws:sts::123456789012:assumed-role/test-role/test 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.invokedBy | string |  |   lambda.amazonaws.com 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.principalId | string |  |   ABCDEFGHI1234567890:test 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.attributes.creationDate | string |  |   2019-07-30T10:47:45Z 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.attributes.mfaAuthenticated | string |  |   false 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.sessionIssuer.accountId | string |  |   123456789012 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.sessionIssuer.arn | string |  `aws arn`  |   arn:aws:iam::123456789012:role/test-role 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.sessionIssuer.principalId | string |  |   ABCDEFGHI1234567890 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.sessionIssuer.type | string |  |   Role 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.sessionContext.sessionIssuer.userName | string |  `user name`  |   testUser 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.type | string |  |   AssumedRole 
action_result.data.\*.ExtractedCloudTrailEvent.userIdentity.userName | string |  `email`  `user name`  |   test@example.us 
action_result.data.\*.ReadOnly | string |  |   true 
action_result.data.\*.Resources.\*.ResourceName | string |  |   ABCDEFGHI123456789 
action_result.data.\*.Resources.\*.ResourceType | string |  |   AWS::IAM::AccessKey 
action_result.data.\*.Username | string |  `email`  `user name`  |   testUser 
action_result.summary.total_lookup_events | numeric |  |   123 
action_result.message | string |  |   Total lookup events: 123 
summary.total_objects | numeric |  |   1 
summary.total_objects_successful | numeric |  |   1 
action_result.parameter.credentials | string |  `aws credentials`  |   {'AccessKeyId': 'ASIASJL6ZZZZZ3M7QC2J', 'Expiration': '2021-06-07 22:28:04', 'SecretAccessKey': 'ZZZZZAmvLPictcVBPvjJx0d7MRezOuxiLCMZZZZZ', 'SessionToken': 'ZZZZZXIvYXdzEN///////////wEaDFRU0s4AVrw0k0oYICK4ATAzOqzAkg9bHY29lYmP59UvVOHjLufOy4s7SnAzOxGqGIXnukLis4TWNhrJl5R5nYyimrm6K/9d0Cw2SW9gO0ZRjEJHWJ+yY5Qk2QpWctS2BGn4n+G8cD6zEweCCMj+ScI5p8n7YI4wOdvXvOsVMmjV6F09Ujqr1w+NwoKXlglznXGs/7Q1kNZOMiioEhGUyoiHbQb37GCKslDK+oqe0KNaUKQ96YCepaLgMbMquDgdAM8I0TTxUO0o5ILF/gUyLT04R7QlOfktkdh6Qt0atTS+xeKi1hirKRizpJ8jjnxGQIikPRToL2v3ZZZZZZ=='} 