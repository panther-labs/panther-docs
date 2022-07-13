---
description: >-
  Manage detections and schemas in Panther with a CI/CD workflow using GitHub
  Actions
---

# GitHub Actions Onboarding Guide

## **Overview**

You can configure GitHub Actions to automate testing, customize detections, and upload your detection pipeline from your GitHub repository to your Panther Console. This guide will walk you through the following:

* Creating a custom workflow via GitHub Actions
* Testing your custom schemas and detections
* Uploading the schemas and detections to your Panther Console
* Customizing your GitHub Actions workflow to fit your organization's needs

## Configure **GitHub Actions for Panther**

### Prerequisites

Before finalizing GitHub Actions with Panther, please reach out to Panther's Support team to get your Panther Console set up with the `PantherAnalysisFederatedCDRole` needed to assume the role directly using the GitHub OIDC provider.&#x20;

### **Build a GitHub workflow to test schemas, detections, and upload to Panther**

1. Navigate to the GitHub repository where you would like to set up automation.
2. Within the GitHub repository, navigate to **Actions.**\
   ****<img src="../.gitbook/assets/Screen Shot 2022-06-14 at 8.51.10 AM (1).png" alt="" data-size="original">****
3. Click **New Workflow**.\
   ![](<../.gitbook/assets/Screen Shot 2022-06-14 at 9.43.19 AM.png>)
4. Click the button that says _**set up a workflow yourself →**._\
   __![](<../.gitbook/assets/Screen Shot 2022-06-14 at 9.49.27 AM.png>)__
5. On the next page, replace the default filename (`main.yml`) with a memorable name, e.g., `panther-workflow.yml`.
6. &#x20;Customize your workflow as follows:
   * **Name:** Create a memorable name.
   *   **Permissions:** Add a permissions field to the workflow that mirrors the below:\
       name: GitHub Actions CI/CD workflow

       ```
       name: GitHub Actions CI/CD workflow

       permissions:
         id-token: write
         contents: read
       ```
   *   **Replace** the default `on` section with the following:

       ```
       on:  
         push:
           paths:
             - 'detections/**'
       ```

       * This defines how to control the workflow to trigger when users perform a git push to the specific folder path `detections/**`.
       * For other ways to trigger a workflow, please see GitHub's documentation on [using filters](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow#using-filters).
   * **Jobs:** modify the jobs sections as follows:
     *   **Modify** the first job downloads, aka the `pantherlog` tool we use to perform schema tests:

         ```
         jobs: 
            download_pantherlog_tool:
             runs-on: ubuntu-latest
             name: Downloading the pantherlog tool
             steps: 
               - name: Download pantherlog & unzip 
                 run: curl -sSO "https://panther-community-us-east-1.s3.amazonaws.com/v1.32.4/tools/linux-amd64-pantherlog.zip" && unzip linux-amd64-pantherlog.zip
               - name: Create a pantherlog artifact
                 uses: actions/upload-artifact@v3
                 with:
                   name: pantherlog
                   path: pantherlog
                   retention-days: 1
         ```


     *   **Add a job** that runs schema tests using the `pantherlog` tool:

         ```
         run_schema_tests:    
             runs-on: ubuntu-latest
             name: Run schema tests with pantherlog
             needs: [download_pantherlog_tool]
             steps:
               - name: Check out the repo
                 uses: actions/checkout@v2
               - name: Download Pantherlog tool from artifacts
                 uses: actions/download-artifact@v3
                 with: 
                   name: pantherlog
               - name: Make pantherlog executable
                 run: sudo chmod +x pantherlog
               - name: Perform schema tests with pantherlog
                 run: ./pantherlog test detections/schemas
         ```


     *   **Add a job** to run unit tests using the panther\_analysis\_tool: &#x20;

         ```
         run_unit_tests:    
             runs-on: ubuntu-latest
             name: Unit Testing with panther_analysis_tool
             needs: [download_pantherlog_tool, run_schema_tests]
             steps:
               - name: Check out the repo
                 uses: actions/checkout@v2
               - name: Download the panther_analysis_tool
                 run: pip3 install panther_analysis_tool
               - name: Run unit tests within the Detections folder
                 run: |
         	  for dir in detections/*; do
         	    if [[ "$dir" =~ .*_rules.* ]]; then
         	      panther_analysis_tool test
         	    fi
         	  done
         ```


     * **Add the last job** to upload detections and custom schemas to the Panther Console. This leverages a pre-built action called [configure-aws-credentials@v1](https://github.com/aws-actions/configure-aws-credentials) to assume a role directly using GitHub OIDC provider.&#x20;
       *   **Note**: Make sure you **replace** the **AWS Account ID** as well as the **AWS Region** with the values matching your instance of Panther. \


           ```
           panther_analysis_tool_upload:        
               runs-on: ubuntu-latest
               name: panther_analysis_tool upload to panther console
               needs: [download_pantherlog_tool, run_schema_tests, run_unit_tests]
               steps:
                 - name: Checkout the repo
                   uses: actions/checkout@v2
                 - name: Configure AWS credentials leveraging OIDC to make the connection
                   uses: aws-actions/configure-aws-credentials@v1 # https://github.com/aws-actions/configure-aws-credentials
                   with:
                     role-to-assume: arn:aws:iam::1234567891012:role/PantherAnalysisFederatedCDRole # Replace with your Panther AWS Account ID
                     aws-region: us-west-2 # Replace with AWS region your Panther instance is in 
                 - name: Download panther_analysis_tool
                   run: pip3 install panther_analysis_tool
                 - name: Loop through folders ending in _rules and upload to your Panther instance 
                   run: |
                     for dir in detections/*; do
                       if [[ "$dir" =~ .*_rules.* ]]; then
                         panther_analysis_tool upload --path "$dir" --skip-tests
                       fi
                     done
                 - name: Upload custom schemas to Panther Console
           	run: panther_analysis_tool update-custom-schemas --path schemas/

           ```

### Pushing detections via GitHub Actions

Now that the GitHub Actions workflow is complete, the following will occur the next time you use `git push` to make changes within the `detections/` folder:

* Custom log schemas are tested with `pantherlog`.
* Custom detections are tested with `panther_analysis_tool`.
* Upon success, schema and detections are uploaded to your Panther Console.

For reference, the full GitHub CI/CD GitHub Actions workflow schema is aggregated below:

<details>

<summary>Complete GitHub CI/CD workflow in one schema</summary>

```yaml
name: Github Actions CI/CD workflow

permissions:
  id-token: write
  contents: read

on:  
  push:
    paths:
      - 'detections/**'

jobs: 
  download_pantherlog_tool:
    runs-on: ubuntu-latest
    name: Downloading the pantherlog tool
    steps: 
      - name: Download pantherlog & unzip 
        run: curl -sSO "https://panther-community-us-east-1.s3.amazonaws.com/v1.32.4/tools/linux-amd64-pantherlog.zip" && unzip linux-amd64-pantherlog.zip
      - name: Create a pantherlog artifact
        uses: actions/upload-artifact@v3
        with:
          name: pantherlog
          path: pantherlog
          retention-days: 1
  run_schema_tests:    
    runs-on: ubuntu-latest
    name: Run schema tests with pantherlog
    needs: [download_pantherlog_tool]
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2
      - name: Download Pantherlog tool from artifacts
        uses: actions/download-artifact@v3
        with: 
          name: pantherlog
      - name: Make pantherlog executable
        run: sudo chmod +x pantherlog
      - name: Perform schema tests with pantherlog
        run: ./pantherlog test detections/schemas
  run_unit_tests:    
    runs-on: ubuntu-latest
    name: Unit Testing with panther_analysis_tool
    needs: [download_pantherlog_tool, run_schema_tests]
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2
      - name: Download the panther_analysis_tool
        run: pip3 install panther_analysis_tool
      - name: Run unit tests within the Detections folder
        run: |
	  for dir in detections/*; do
	    if [[ "$dir" =~ .*_rules.* ]]; then
	      panther_analysis_tool test
	    fi
	  done
  panther_analysis_tool_upload:        
    runs-on: ubuntu-latest
    name: panther_analysis_tool upload to panther console
    needs: [download_pantherlog_tool, run_schema_tests, run_unit_tests]
    steps:
      - name: Checkout the repo
        uses: actions/checkout@v2
      - name: Configure AWS credentials leveraging OIDC to make the connection
        uses: aws-actions/configure-aws-credentials@v1 # https://github.com/aws-actions/configure-aws-credentials
        with:
          role-to-assume: arn:aws:iam::1234567891012:role/PantherAnalysisFederatedCDRole # Replace with your Panther AWS Account ID
          aws-region: us-west-2 # Replace with AWS region your Panther instance is in 
      - name: Download panther_analysis_tool
        run: pip3 install panther_analysis_tool
      - name: Loop through folders ending in _rules and upload to papaya-oarfish 
        run: |
          for dir in detections/*; do
            if [[ "$dir" =~ .*_rules.* ]]; then
              panther_analysis_tool upload --path "$dir" --skip-tests
            fi
          done
      - name: Upload custom schemas to Panther Console
	run: panther_analysis_tool update-custom-schemas --path schemas/
```

</details>

### Customize your GitHub Actions workflow in Panther

Optionally, you can extend or customize this workflow to better fit your organization. The following are common workflow customizations with Panther:

* Perform Python Linting against `.py` files.
* Trigger from an approved Pull Request (PR) instead of a Push to a specific folder.
* If you fork the [panther-analysis](https://github.com/panther-labs/panther-analysis) repository by the latest tag, learn how [syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) can help keep Panther Detections up-to-date. We recommend syncing weekly by tag.

{% hint style="info" %}
Additional GitHub Actions documentation can be found [here](https://docs.github.com/en/actions).&#x20;
{% endhint %}
