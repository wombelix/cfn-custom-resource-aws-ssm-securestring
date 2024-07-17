<!--
SPDX-FileCopyrightText: 2024 Dominik Wombacher <dominik@wombacher.cc>

SPDX-License-Identifier: MIT
-->

# dotfiles

A custom resource for CloudFormation to interact with SecureString values from AWS SSM Parameter Store. 
I came up with that while creating and managing AWS infrastructure for some of my side-projects.

[![REUSE status](https://api.reuse.software/badge/git.sr.ht/~wombelix/cfn-custom-resource-aws-ssm-securestring)](https://api.reuse.software/info/git.sr.ht/~wombelix/cfn-custom-resource-aws-ssm-securestring)

## Table of Contents

* [Example](#example)
* [Source](#source)
* [Contribute](#contribute)
* [License](#license)

## Example

I use the following Cfn snippet to create an entry in AWS SSM Parameter Store, encrypted with an AWS KMS key. 
The different values coming from Parameters or other resources from my CloudFormation Stack. 

`ServiceToken` is the `arn` of the custom resource Lambda function that will be triggered.

The rest should be self-explaining, `Name`, `Value` and `Description` will be added to the Parameter Store entry. 
`KmsKeyId` is the `arn` of the AWS KMS key you want to use to encrypt the the entry.

```
  IAMUserIACOpenTofuAccessKeyParameterStore:
    Type: AWS::CloudFormation::CustomResource
    Properties:
      ServiceToken: !Sub arn:${AWS::Partition}:lambda:${AWS::Region}:${AWS::AccountId}:function:${CustomResourceLambdaName}
      Name: !Ref ParameterNameForAccessKey
      Value: !Ref IAMUserIACOpenTofuAccessKey
      Description: !Sub "The access key for IAM User ${IAMUserIACOpenTofu}"
      KmsKeyId: !ImportValue
                  'Fn::Sub': ${KMSStackName}-KMSKeyBackendEncryptionArn
      Tags: 
        - Key: "Environment"
          Value: "Production"
        - Key: "Usage"
          Value: "IAC-OpenTofu"
```

## Source

The primary location is: https://git.sr.ht/~wombelix/cfn-custom-resource-aws-ssm-securestring

<!--
Mirrors of the repository are available on
[Codeberg](https://codeberg.org/wombelix/cfn-custom-resource-aws-ssm-securestring),
[Gitlab](https://gitlab.com/wombelix/cfn-custom-resource-aws-ssm-securestring) and
[Github](https://github.com/wombelix/cfn-custom-resource-aws-ssm-securestring).
-->

## Contribute

Please don't hesitate to provide Feedback, open an Issue or create an Pull / Merge Request.

Just pick the workflow or platform you prefer and are most comfortable with.

Feedback, bug reports or patches to my sr.ht list [~wombelix/inbox@lists.sr.ht](https://lists.sr.ht/~wombelix/inbox)
or via [Email or Instant Messaging](https://dominik.wombacher.cc/pages/contact.html) are also always welcome.

## License

Unless otherwise stated: `MIT`

All files contain license information either as `header comment` or `corresponding .license` file.

[REUSE](https://reuse.software) from the [FSFE](https://fsfe.org/) implemented to verify license and copyright compliance.

