## Deploying S3 Bucket Template Using Only AWS Native Tools

This document describes how to launch the S3 Bucket template using only native AWS tools

### AWS CLI

The AWS CLI can be used to launch the template using either a locally-staged copy of the template or an S3-hosted copy of the template. In either case it will be necessary to supply template-parameters. While this can be done directly within the CLI, readability and repeatability are aided by using a parameters file. The parameters file will resemble the following:

~~~~
[
    {
        "ParameterKey": "JenkinsBackupBucket",
        "ParameterValue": "my-s3-bukkit"
    }
]
~~~~

Or

~~~~
[
    {
        "ParameterKey": "JenkinsBackupBucket",
        "ParameterValue": ""
    }
]
~~~~

Note that, in the second example, the `ParameterValue` is null. Specifying a name for the S3 bucket is optional. If this value is left as null, AWS will programatically determine a name for the bucket.

Done directly within the cli, the required parameters-enumerations would look like:

~~~~
--parameters '[{"ParameterKey":"JenkinsBackupBucket","ParameterValue":"my-s3-bukkit"}]'
~~~~

Or

~~~~
--parameters '[{"ParameterKey":"JenkinsBackupBucket","ParameterValue":""}]'
~~~~

While merely clunky for a simple template like the S3 Bucket template, it becomes progressively more clunky as the number of parameters in a template increases.

The following sub-sections assume:
- The template user has previously used `aws configure` to set a default region for their commands to run within. If this is not the case, it will be necessary to insert the `--region <REGION_NAME>` option to the CLI command
- The template user is not attempting to execute the command against a non-default account-profile. If this is not the case, it will be necessary to insert the `--profile <PROFILE_NAME>` option to the CLI command

#### Locally Hosted Template

When referencing a locally-hosted file, one uses the `--template-body` parameter. Depending on the OS type being launched from specifying the path to the template may require using the `file://` URI. Similarly, if using a parameters file, one may need to use the `file://` URI. The resulting command will look like:

~~~~
aws cloudformation create-stack --stack-name make_jenkins_S3-bucket.tmplt.json \
  --template-body file:///<FULLY_QUALIFIED>/<PATH>/<TO>/<TEMPLATE> \
  --parameters file:///<FULLY_QUALIFIED>/<PATH>/<TO>/<PARAMETER_FILE>
~~~~

Or:

~~~~
aws cloudformation create-stack --stack-name make_jenkins_S3-bucket.tmplt.json \
  --template-body file://<RELATIVE>/<PATH>/<TO>/<TEMPLATE> \
  --parameters file://<RELATIVE>/<PATH>/<TO>/<PARAMETER_FILE>
~~~~

The latter saves typing if the template- and parameter-files are either co-located or proximal to each other within the local filesystem. This can mean the difference between typing:

`file:///home/myuser/GIT/cfn-jenkins/Templates/make_jenkins_S3-bucket.tmplt.json`

and typing:

`file://make_jenkins_S3-bucket.tmplt.json`

#### S3-Hosted Template

When referencing a S3-hosted file, one uses the `--template-url` parameter. If using a parameters file, one may need to use the `file://` URI. The resulting command will look like:

~~~~
aws cloudformation create-stack --stack-name make_jenkins_S3-bucket.tmplt.json \
  --template-url https://s3.<REGION>.<AMAZON_TLD>/<BUCKET_NAME>/<TEMPLATE_NAME> \
  --parameters file:///<FULLY_QUALIFIED>/<PATH>/<TO>/<PARAMETER_FILE>
~~~~

Or:

~~~~
aws cloudformation create-stack --stack-name make_jenkins_S3-bucket.tmplt.json \
  --template-url https://s3.<REGION>.<AMAZON_TLD>/<BUCKET_NAME>/<TEMPLATE_NAME> \
  --parameters file://<RELATIVE>/<PATH>/<TO>/<PARAMETER_FILE>
~~~~

### AWS Web UI

(TBD)

