# Development

Panther is a collection of serverless applications deployed on an AWS instance managed by Panther. The frontend is a React application which runs in a Docker container \(via ECS\), and the backend is a collection of compute \(Lambda\), storage \(DynamoDB / S3\), and other supporting services.

The sections below cover how Panther works under the hood: an architectural overview and how your data is stored/processed.

## Architecture Diagram

This diagram provides an overview of the core components of Panther, and how they are connected.

![High level architecture diagram](../.gitbook/assets/development-arch-diagram%20%288%29%20%288%29%20%289%29%20%281%29.png)

### Data Flow Diagram

This diagram shows where and how your data is stored and processed:

![Data flow diagram](../.gitbook/assets/development-data-flow-diagram%20%288%29%20%288%29%20%281%29%20%286%29.png)

The above arrows indicate the direction in which data is transferred, as opposed to the previous diagrams where arrows are indicating the direction that communication is being initiated.

