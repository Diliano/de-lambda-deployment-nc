# Demonstration Project For AWS Infrastructure

The purpose of this project is to demonstrate (in a manner close to production-level code) how to deploy a simple AWS Lambda and associated infrastructure to an AWS account.

Features of the project:
1. A simple lambda handler (`src/file_reader/reader.py`) that accepts an S3 event, checks if it refers to a `.txt` file, and then logs the contents of the file.
1. The code features trapping of specific errors (including a custom error) and handles unexpected RuntimeErrors.
1. The code is tested (`test/test_file_reader/test_lambda.py`) using the `moto` library to mock AWS artefacts. 
1. The project build is via a `Makefile` which allows `bandit` and `safety` checks for security vulnerabilities, and `black` checks for PEP8 compliance.
1. The application can be deployed via Terraform.


### Prerequisites for local development
- Python
- Make
- Terraform

### __Instructions for Students__
1. Fork and clone this project. 
2. In the terminal, navigate to the root directory of the project, and run:
    ```bash
    make requirements
    ```
3. Then run:
    ```bash
    make dev-setup
    make run-checks
    ```
4. This Terraform code is set up with a remote state. However, an S3 bucket needs to exist in your own AWS account to store that state.
   - Use the AWS CLI to create a bucket with a unique name
   - Update the value `bucket` attribute of the `backend "s3" {` block in `./terraform/main.tf` to the name of the bucket you've just created.

6. Within the `terraform` directory, check the terraform deployment by running:
    ```bash
    terraform init
    terraform plan
    ```
   __But please do *not* deploy the code from your local machine.__

### Tasks
In the `deploy.yml` file in the `.github` directory: 
1. Write a job that will run all the tests in the project, including unit tests, linting, security, and coverage. The job should run on a standard Ubuntu worker. The job should be triggered on the code 
being pushed to GitHub.
1. Write a job that will run _if the tests succeed_ that will deploy the Lambda and the 
associated infrastructure using Terraform.
