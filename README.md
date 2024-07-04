# CI/CD Pipeline Project with AWS CodePipeline

This project demonstrates the implementation of a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Python test application using AWS CodePipeline. The goal is to automate the process of integrating code changes and deploying them to production environments efficiently.

## Overview

The CI/CD pipeline is designed to automatically build, test, and deploy the Flask application whenever changes are pushed to the source code repository. This ensures that the application is always in a deployable state, reducing manual intervention and increasing deployment frequency.

## Technologies Used

- **Python**: The primary programming language used for developing the Flask application.
- **AWS CodePipeline**: Orchestrates the build, test, and deploy phases of the release process every time there is a code change.
- **AWS CodeBuild**: Compiles source code, runs tests, and produces software packages that are ready to deploy.
- **AWS CodeDeploy**: Automates code deployments to any instance, including Amazon EC2 instances and on-premises servers.