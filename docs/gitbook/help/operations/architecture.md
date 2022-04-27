# Architecture

Panther is a collection of server-less applications deployed on an AWS instance managed by Panther. The frontend is a React application which runs in a Docker container (via ECS), and the backend is a collection of compute (Lambda), storage (DynamoDB / S3), and other supporting services.

## Supported AWS Regions

Panther can be deployed to any of the following regions:

* `ap-northeast-1` (Tokyo)
* `ap-south-1` (Mumbai)
* `ap-southeast-1` (Singapore)
* `ap-southeast-2` (Sydney)
* `eu-central-1` (Frankfurt)
* `eu-west-1` (Ireland)
* `eu-west-2` (London)
* `us-east-1` (N. Virginia)
* `us-east-2` (Ohio)
* `us-west-2` (Oregon)

## Architecture Diagram

This diagram provides an overview of the core components of Panther, and how they are connected.

![High level architecture diagram](<../../../../.gitbook/assets/development-arch-diagram (8) (8) (9) (6) (1) (1) (2) (1) (9).png>)

### Data Flow Diagram

This diagram shows where and how your data is stored and processed:

![Data flow diagram](<../../../../.gitbook/assets/development-data-flow-diagram (8) (8) (1) (1) (1) (2) (1) (9).png>)

The above arrows indicate the direction in which data is transferred, as opposed to the previous diagrams where arrows are indicating the direction that communication is being initiated.
