# Cost

This page offers some insight related to the costs of running Panther. Costs will vary across deployments based on a number of factors, including data processing volumes and data retention lengths.

## Overview

Panther is proudly built with modern, serverless technologies that is resource-efficient at scale. As a result, Panther is cost-effective and should run within AWS's Free Tier until you start onboarding data to analyze. Some infrastructure Panther relies on has an ongoing cost regardless of usage. For example, Panther creates a custom KMS key for SQS encryption, which has a fixed cost of $1/month. The main cost comes from running the web application continuously with ECS Fargate, continually analyzing data with Lambda, and storing parsed logs in S3.

## Frontend

To serve the web application, an ECS Fargate service named `panther-web` runs a single task for the front-end server. By default, this task is allocated 0.5 of vCPU and 1024MB of memory, which leads to a monthly cost of $14.57 \(vCPU\) + $3.2 \(RAM\) = $17.77 according to the [official ECS pricing page](https://aws.amazon.com/fargate/pricing/).

This means that the minimum cost of running the container is **$17.77/month**. If you want to lower this cost in exchange for a slower server and an increased load time, you can modify the parameters found in the [panther\_config.yml](https://github.com/panther-labs/panther-enterprise/blob/master/deployments/panther_config.yml).

Specifically, you can make the following changes:

* Set `WebApplicationFargateTaskCPU` to `256`
* Set `WebApplicationFargateTaskMemory` to `512`

Then, deploy \(or redeploy\) Panther.

These values are the minimum allowed values for the front-end, and they will decrease the cost down to **$8.88/month**.

## Log Processing

A majority of Panther's log analysis cost comes from S3 storage. Because of Panther's design, compute cost is minimal and avoids usage of expensive services. Actual costs will vary based on factors including volume of data processed and data retention lengths.

