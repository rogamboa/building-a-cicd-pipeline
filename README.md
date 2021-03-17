# Building a CI/CD Pipeline

## Project Summary
This project demonstrates an established Continuous Integration/Continuous Deployment (CI/CD) pipeline using CircleCI and deployment to an AWS Cloud environment.

The web application used in this project is based on the following sample code provided [here](https://github.com/udacity/cdond-c3-projectstarter).

Note: The web app utilizes a PostgreSQL server and a CloudFront distribution that are pre-deployed outside of the pipeline.

## Project Contents
- ```.circleci``` - contains files used for CI/CD pipeline configuration
  -  ```config.yml``` - CircleCI configuration file
  -  ```ansible``` - contains files for ansible deployment jobs
    -  ```configure-server.yml``` - ansible playbook to configure backend instance
    -  ```deploy-backend.yml``` - ansible playbook to deploy backend web app files
    -  ```inventory.txt``` - ansible inventory file used to track backend instance IP
    -  ```roles``` - contains ansible roles
  -  ```files``` - contains AWS Cloudformation templates for infrastructure deployment
    -   ```backend.yml``` - creates an EC2 instances for web app backend
    -   ```cloudfront.yml``` - creates a CloudFront distribution
    -   ```frontend.yml``` - creates an S3 bucket for web app frontend
- ```backend``` - contains backend web app files
- ```frontend``` - contains frontend web app files

## Pipeline Workflow
Workflow found in ```config.yml```
### Steps
#### Build Phase
- ```build-frontend```
  - Builds frontend web app files
- ```build-backend```
  - Builds backend web app files
- ```test-frontend```
  - Performs tests on frontend code
- ```test-backend```
  - Performs tests on backend code
- ```scan-frontend```
  - Security scans frontend web app files
- ```scan-backend```
  - Security scans backend web app files
#### Deploy Phase
The following will only execute on when a commit is made on the ```master``` branch.
- ```deploy-infrastructure```
  - Deploys an S3 bucket (for frontend) and EC2 instance (for backend) to AWS via CloudFormation
- ```configure-infrastructure```
  - Configures backend EC2 instance with packages necessary for deployment via ansible
- ```run-migrations```
  - Runs database migrations on PostgreSQL server for backend server use
- ```build-frontend-deployment```
  - Builds frontend web app files for deployment
- ```build-backend-deployment```
  - Builds backend web app files for deployment
- ```deploy-frontend```
  - Deploys frontend web app files to S3 bucket via AWS ClI
- ```deploy-backend```
  - Deploys frontend web app files to S3 bucket via ansible
- ```smoke-test```
  - Performs curl test on frontend and backend to confirm succesful deployment
- ```cloudfront-update```
  - Updates CloudFront distribution to point to new web app deployment
- ```cleanup```
  - Cleans up old web app deplyoment created prior to current deployment workflow
