<img src="/images/awsiam/awsiam.png" />
## AWS IAM Integration
The Next Level3 AWS IAM integration is designed to be used for your existing applications or sites that are using AWS IAM for authentication. This integration will allow you to easily add Account Protection to any application that leverages AWS IAM for authentication. 

Pre-requisites
  * AWS Account Leveraging IAM Users or Roles*
  * Next Level3 Company Account
  * Signing Key created for an application in the Next Level3 Company Portal
### Account Protection
The AWS IAM integration is slightly different from other integrations. AWS does not provide access to the login flow for IAM users or roles so there is no way to directly implement the lock check. However, via Amazon EventBridge, we can trigger a lock check on login and, if the account is locked, apply a “Deny All” managed policy, a “Revoke Sessions” policy, and even disable any active access keys. If the account is unlocked, the event would ensure that the policies are removed and the access keys that were most recently used are re-activated. Please be aware that there is a small delay between the login event and any policy being applied or removed. Also, if the Amazon EventBridge event is not triggered correctly, the lock check will not be performed and no policy will be applied.

The first step for setting up this integration is to create a Lambda function that can perform the lock check when a login event occurs and apply or remove the policies. Here is some sample code for a regular IAM user written in Python:
```Python
import json
import os
import requests
import base64
import logging
from datetime import datetime
import time
import jwt
import boto3
from botocore.exceptions import ClientError

iamClient = boto3.client('iam',region_name="us-east-1")

logger = logging.getLogger()
logger.setLevel(logging.INFO)

policyDocument = "{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Action\": [ \"*\" ], \"Effect\": \"Deny\", \"Resource\": \"*\" } ] }"
awsRevokeOlderSessionsPolicyDocument = "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [{\n    \"Effect\": \"Deny\",\n    \"Action\": [ \"*\" ],\n    \"Resource\": [ \"*\" ],\n    \"Condition\": {\n      \"DateLessThan\": {\n\"aws:TokenIssueTime\": \"" + datetime.now().strftime("%Y-%m-%dT%H:%M:%S.000Z") + "\"\n      }\n    }\n  }\n]}"

def restoreAccess(userName):
    try:
        paginator = iamClient.get_paginator('list_access_keys')
        keysExist = False
        keyUsage = []
        for accessKey in paginator.paginate(UserName=userName):
            keysExist = True
            lastUsed = iamClient.get_access_key_last_used(
                AccessKeyId = accessKey["AccessKeyMetadata"][0]["AccessKeyId"]
            )

            keyUsage.append({ "AccessKeyId": accessKey["AccessKeyMetadata"][0]["AccessKeyId"], "AccessKeyLastUsed": (time.mktime(lastUsed["AccessKeyLastUsed"]["LastUsedDate"].timetuple())) if "LastUsedDate" in lastUsed["AccessKeyLastUsed"].keys() else 9999999999999 })
        if keysExist:
            keyUsage.sort(key = lambda x: x["AccessKeyLastUsed"])
            iamClient.update_access_key (
                AccessKeyId=keyUsage[0]["AccessKeyId"],
                Status='Active',
                UserName=userName
            )
        response = iamClient.detach_user_policy(
            UserName=userName,
            PolicyArn=os.environ["POLICY_ARN"]
        )
    except ClientError as error:
        if error.response["Error"]["Code"] == "NoSuchEntity":
            restoreAccessByInlinePolicy(userName)
        else:
            raise error

def restoreAccessByInlinePolicy(userName):
    try:
        response = iamClient.delete_user_policy(
            UserName=userName,
            PolicyName="DenyAllAccess"
        )
    except ClientError as error:
        raise error

def revokeAccess(userName):
    try:
        response = iamClient.put_user_policy(
            UserName=userName,
            PolicyName="AwsRevokeOlderSessions",
            PolicyDocument=awsRevokeOlderSessionsPolicyDocument
        )
        paginator = iamClient.get_paginator('list_access_keys')
        for accessKey in paginator.paginate(UserName=userName):
            logger.info(str(accessKey))
            if accessKey["AccessKeyMetadata"][0]["Status"] == 'Active':
                iamClient.update_access_key (
                    AccessKeyId=accessKey["AccessKeyMetadata"][0]["AccessKeyId"],
                    Status='Inactive',
                    UserName=userName
                )
        response = iamClient.attach_user_policy(
            UserName=userName,
            PolicyArn=os.environ["POLICY_ARN"]
        )
    except ClientError as error:
        if error.response["Error"]["Code"] == "LimitExceeded":
            revokeAccessByInlinePolicy(userName)
        else:
            raise error

def revokeAccessByInlinePolicy(userName):
    try:
        response = iamClient.put_user_policy(
            UserName=userName,
            PolicyName="DenyAllAccess",
            PolicyDocument=policyDocument
        )
    except ClientError as error:
        raise error

def getLockStatus(token, api_uri, api_path, event):
  responseDict = {}
  try:
    headers_dict = {"x-nl3-authorization-token": token}
    responseIPInfo = {}
    location = ""
      if "." in event["detail"]["sourceIPAddress"] or ":" in event["detail"]["sourceIPAddress"]:
        responseIPInfo = requests.get("https://ipinfo.io/" + event["detail"]["sourceIPAddress"] + "?token=[ipinfo_token]").json()
        if "city" in responseIPInfo:
            location = responseIPInfo["city"] + ", " + responseIPInfo["region"]
    data_dict = {
      "userIP": event["detail"]["sourceIPAddress"],
      "userDevice": event["detail"]["userAgent"],
      "userLocation": location,
      "integrationType": "awsiamuser",
      "integrationData": responseIPInfo
    }
    response = requests.post("".join([api_uri,api_path]), headers=headers_dict, json=data_dict)
    responseDict = response.json()
  except Exception as e:
    responseDict = { "message": str(e) }

  return responseDict

def lambda_handler(event, context):
    userName = event["detail"]["userIdentity"]["userName"]
    claims = {
                "iss": os.environ["APP_URI"],
        "iat": (datetime.utcnow().timestamp() + (-1 * 60)),
        "exp": (datetime.utcnow().timestamp() + (5 * 60)),
        "aud": os.environ["API_URI"],
        "sub": userName
    }
    ### Ideally the Signing Key would be stored and retrieved from a secrets manager
    ### and not an environmental variable
    decodedDomainToken = base64.b64decode(os.environ["SIGNING_KEY"]);
    token = jwt.encode(
        payload=claims,
        key=decodedDomainToken
    )
    response = getLockStatus(token, os.environ["API_URI"], os.environ["API_PATH"], event)
    if response.get("locked", False):
        revokeAccess(userName)
    else:
        restoreAccess(userName)
```
* If you use IAM Roles and not IAM Users, please contact support to discuss the options for implementation and specific design considerations that may be required to implement this correctly using roles.

The next step is to setup the Amazon EventBridge event rule that triggers the Lambda function for specific events. Here is a sample rule:
```Python
{"source":["aws.signin"],"detail-type":["AWS Console Sign In via CloudTrail"],"detail":{"userIdentity":{"type":["IAMUser"],"userName":["list","of","usernames","can","omit","this","line","if","all","users"]},"eventSource":["signin.amazonaws.com"],"eventName":["ConsoleLogin"],"responseElements":{"ConsoleLogin":["Success"]}}}
```

References:

https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html

*If you use IAM Roles and not IAM Users, please contact support to discuss the options for implementation and specific design considerations that may be required to implement this correctly using roles.

The next step is to setup the Amazon EventBridge event rule that triggers the Lambda function for specific events. Here is a sample rule:

References:

https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html