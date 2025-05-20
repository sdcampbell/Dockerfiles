# AWSPEAS Docker Container

This Docker container provides a ready-to-use version of AWSPEAS from the [CloudPEASS](https://github.com/carlospolop/CloudPEASS) suite for AWS privilege escalation detection and enumeration.

## Features

- **Cross-architecture compatibility**: Supports both x86_64 and ARM64 (Apple Silicon) architectures
- **Ready-to-use**: All dependencies pre-installed
- **Configurable**: Customize behavior through environment variables

## Overview

AWSPEAS is a tool designed to find AWS permissions and potential privilege escalation paths without modifying any resources. It leverages multiple techniques to identify permissions:

1. IAM Policy Enumeration - Reviews IAM policies attached to the principal
2. Permission Simulation - Simulates effective permissions of the principal
3. Brute-Force Enumeration - Tests List, Get, and Describe API calls via AWS CLI

## Usage

### Building the Docker Image

```bash
docker build -t awspeas .
```

### Running AWSPEAS

You have multiple options to provide AWS credentials to the container:

#### Option 1: Using AWS Environment Variables

```bash
docker run -it --rm \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret_key \
  -e AWS_REGION=us-east-1 \
  awspeas
```

If you're using temporary credentials, include the session token:

```bash
docker run -it --rm \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret_key \
  -e AWS_SESSION_TOKEN=your_session_token \
  -e AWS_REGION=us-east-1 \
  awspeas
```

#### Option 2: Mounting AWS Credentials

```bash
docker run -it --rm \
  -v ~/.aws:/root/.aws \
  -e AWS_PROFILE=your_profile_name \
  -e AWS_REGION=us-east-1 \
  awspeas
```

### Additional Configuration Options

You can customize the AWSPEAS execution with these environment variables:

- `AWS_PROFILE`: AWS profile to use (default: "default")
- `AWS_REGION`: AWS region to scan (default: "us-east-1")
- `AWSPEAS_OUT_JSON_PATH`: Path for output JSON file
- `AWSPEAS_THREADS`: Number of threads to use
- `AWSPEAS_NOT_USE_HACKTRICKS_AI`: Set to "true" to disable HackTricks AI analysis
- `AWSPEAS_DEBUG`: Set to "true" to enable debug output
- `AWSPEAS_AWS_SERVICES`: Comma-separated list of AWS services to check (e.g., "s3,ec2,lambda")

Example with additional options:

```bash
docker run -it --rm \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret_key \
  -e AWS_REGION=us-east-1 \
  -e AWSPEAS_AWS_SERVICES=s3,ec2,lambda,iam \
  -e AWSPEAS_THREADS=10 \
  -e AWSPEAS_NOT_USE_HACKTRICKS_AI=true \
  awspeas
```

## Output

By default, AWSPEAS outputs results to the console. To save results to a file within the container, set the `AWSPEAS_OUT_JSON_PATH` environment variable and mount a volume:

```bash
docker run -it --rm \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret_key \
  -e AWS_REGION=us-east-1 \
  -e AWSPEAS_OUT_JSON_PATH=/data/results.json \
  -v $(pwd)/results:/data \
  awspeas
```

## Resources

- [CloudPEASS GitHub Repository](https://github.com/carlospolop/CloudPEASS)
