# Shared Workflows
This repository is for well curated sharable workflows 

# How To Use
Requirements:
* GitHub Actions Runner

In order to use you only need to import the "workflow_call" setup in an action in your repo.
Example:
```yaml
name: checkov-scan
on:
  workflow_dispatch:
  push:
    branches:
      - 'main'
jobs:
  checkov-job:
    runs-on: ubuntu-latest
    name: checkov-action
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3

      - name: Run Checkov action
        id: checkov
        uses: bridgecrewio/checkov-action@master
        with:
          directory: .
          soft_fail: true # optional: do not return an error code if there are failed checks
          framework: terraform # optional: run only on a specific infrastructure {cloudformation,terraform,kubernetes,all}
          output_format: sarif # optional: the output format, one of: cli, json, junitxml, github_failed_only, or sarif. Default: sarif
          download_external_modules: true # optional: download external terraform modules from public git repositories and terraform registry
          log_level: WARNING # optional: set log level. Default WARNING, DEBUG for all output
```
# Naming
Cloud Specific Items:
* Cloud specific Items should contain "aws" or "azure" as a prefixed in the file name
    * Example:
        * aws-container-build.yaml
            * In theory this would build and deploy a container to an AWS service such as ECR , EKS etc ...
* Generic workflows that do a particular task that are not tied to a cloud provider are simpled named as their purpose
    * Example :
        * checkov-scan.yaml
            * Will allow you to scan multiple frameworks for security and over all best practices no matter the target infrastructure

# How to contribute

Addendum:
* Workflows should be a generic and customizable as possible
    * Please use common sense and don't introduce complexity that could be solved otherwise by multiple workflows
* All input variables must have description
* Production systems should always use the "releases"
* All input for "workflow_call" must be present as well on "workflow_dispatch"
* Note that the option type is not available for "workflow_call" as in dispatch, please use string in this case
    * Workflows must have a description at the top of the file explaining in plain terms what the workflow does
      Example:
      ```yaml
      # Description:
      # This pipeline will build scan and deploy an container to an ECR repository
      # with the current githash short and the latest tag if selected
      ```