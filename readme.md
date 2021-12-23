[comment]: # "Auto-generated SOAR connector documentation"
# AWS CloudTrail

Publisher: Splunk  
Connector Version: 2\.2\.5  
Product Vendor: AWS  
Product Name: CloudTrail  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 4\.9\.39220  

This app integrates with AWS CloudTrail to perform various investigative actions

[comment]: # " File: readme.md"
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
**use\_role** |  optional  | boolean | Use attached role when running Phantom in EC2

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using the supplied configuration  
[describe trails](#action-describe-trails) - Retrieve settings for trails associated with the current region and the multi\-region trails  
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
Retrieve settings for trails associated with the current region and the multi\-region trails

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**include\_shadow\_trails** |  optional  | Inform command to include shadow trails | boolean | 
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.status | string | 
action\_result\.parameter\.include\_shadow\_trails | boolean | 
action\_result\.data\.\*\.HasCustomEventSelectors | boolean | 
action\_result\.data\.\*\.HomeRegion | string | 
action\_result\.data\.\*\.IncludeGlobalServiceEvents | boolean | 
action\_result\.data\.\*\.IsMultiRegionTrail | boolean | 
action\_result\.data\.\*\.IsOrganizationTrail | boolean | 
action\_result\.data\.\*\.LogFileValidationEnabled | boolean | 
action\_result\.data\.\*\.Name | string | 
action\_result\.data\.\*\.S3BucketName | string | 
action\_result\.data\.\*\.SnsTopicARN | string |  `aws arn` 
action\_result\.data\.\*\.SnsTopicName | string |  `aws arn` 
action\_result\.data\.\*\.TrailARN | string |  `aws arn` 
action\_result\.summary\.message | string | 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 
action\_result\.parameter\.credentials | string |  `aws credentials`   

## action: 'run query'
Lookup the management events captured by CloudTrail

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**attribute\_key** |  optional  | Select an Attribute to query by \(or leave blank to retrieve all records\) | string | 
**attribute\_value** |  optional  | Specify the Value by which to search\. Note that true/false values must be lower\-case | string | 
**start\_date** |  optional  | Start date in the format of yyyy\-mm\-dd \(e\.g\. 2019\-12\-25\) | string | 
**end\_date** |  optional  | End date in the format of yyyy\-mm\-dd \(e\.g\. 2019\-12\-25\) | string | 
**max\_results** |  optional  | Max results to return | numeric | 
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.status | string | 
action\_result\.parameter\.attribute\_key | string | 
action\_result\.parameter\.attribute\_value | string | 
action\_result\.parameter\.end\_date | string | 
action\_result\.parameter\.max\_results | numeric | 
action\_result\.parameter\.start\_date | string | 
action\_result\.data\.\*\.AccessKeyId | string | 
action\_result\.data\.\*\.EventId | string | 
action\_result\.data\.\*\.EventName | string | 
action\_result\.data\.\*\.EventSource | string | 
action\_result\.data\.\*\.EventTime | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.apiVersion | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.awsRegion | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.errorCode | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.errorMessage | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventID | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventSource | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventTime | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventType | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.eventVersion | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.readOnly | boolean | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.recipientAccountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestID | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.Filters\.UpdatedAt\.\*\.End | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.Filters\.UpdatedAt\.\*\.Start | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.MaxResults | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.accountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.agentName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.agentStatus | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.agentVersion | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.computerName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.defaultOnly | boolean | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.encryptionContext\.aws\:lambda\:FunctionArn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.endTime | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.environmentId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.externalId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.fileSystemId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.iPAddress | string |  `ip` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.instanceId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.limit | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.lookupAttributes\.\*\.attributeKey | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.lookupAttributes\.\*\.attributeValue | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.marker | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.maxItems | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.maxRecords | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.maxResults | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.mountTargetId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.networkInterfaceId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.platformName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.platformType | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.platformVersion | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.policyArn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.requestContext\.awsAccountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.resource | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.resourceArn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.resourceId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.roleArn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.roleName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.roleSessionName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.stackName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.startTime | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.topicArn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.requestParameters\.versionId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.resources\.\*\.ARN | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.resources\.\*\.accountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.resources\.\*\.type | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.Message | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.assumedRoleUser\.arn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.assumedRoleUser\.assumedRoleId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.credentials\.accessKeyId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.credentials\.expiration | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.credentials\.sessionToken | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.alias | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.customerUserName | string |  `user name` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.directoryId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.directoryName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.directoryType | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.dnsIpAddresses | string |  `ip` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.iamRoleId | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.registrationCode | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.state | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.subnetIds | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.workspaceCreationProperties\.enableInternetAccess | boolean | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.workspaceCreationProperties\.enableWorkDocs | boolean | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.workspaceCreationProperties\.userEnabledAsLocalAdministrator | boolean | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.directories\.\*\.workspaceSecurityGroupId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.tags\.aws\:cloudformation\:logical\-id | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.tags\.aws\:cloudformation\:stack\-id | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.tags\.aws\:cloudformation\:stack\-name | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.bundleId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.computerName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.directoryId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.ipAddress | string |  `ip` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.state | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.subnetId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.userName | string |  `user name` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceProperties\.computeTypeName | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceProperties\.rootVolumeSizeGib | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceProperties\.runningMode | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceProperties\.runningModeAutoStopTimeoutInMinutes | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.responseElements\.workspaces\.\*\.workspaceProperties\.userVolumeSizeGib | numeric | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.sharedEventID | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.sourceIPAddress | string |  `ip` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userAgent | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.accessKeyId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.accountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.arn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.invokedBy | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.principalId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.attributes\.creationDate | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.attributes\.mfaAuthenticated | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.sessionIssuer\.accountId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.sessionIssuer\.arn | string |  `aws arn` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.sessionIssuer\.principalId | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.sessionIssuer\.type | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.sessionContext\.sessionIssuer\.userName | string |  `user name` 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.type | string | 
action\_result\.data\.\*\.ExtractedCloudTrailEvent\.userIdentity\.userName | string |  `email`  `user name` 
action\_result\.data\.\*\.ReadOnly | string | 
action\_result\.data\.\*\.Resources\.\*\.ResourceName | string | 
action\_result\.data\.\*\.Resources\.\*\.ResourceType | string | 
action\_result\.data\.\*\.Username | string |  `email`  `user name` 
action\_result\.summary\.total\_lookup\_events | numeric | 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 
action\_result\.parameter\.credentials | string |  `aws credentials` 