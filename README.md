# Simple Node.js App for Jenkins Docker Pipeline

This is a simple Node.js application demonstrating a Jenkins pipeline that builds and runs a Docker container.

## Setup

1. Ensure you have Node.js and Docker installed.
2. Run `npm install` to install dependencies.
3. To run locally: `npm start`

## Jenkins Pipeline

The `Jenkinsfile` defines a pipeline that:
- Checks out the code
- Builds a Docker image
- Runs the Docker container

To use with Jenkins:
1. Create a new pipeline job in Jenkins.
2. Point it to this repository.
3. Run the pipeline.

## GitHub

This project is set up for GitHub. After initializing git, create a new repository on GitHub and push this code.