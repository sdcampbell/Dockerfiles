FROM python:3.10-slim

# Install git, AWS CLI, and other dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    curl \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Install AWS CLI v2 based on architecture (supports both x86_64 and ARM64)
RUN ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"; \
    else \
        echo "Unsupported architecture: $ARCH"; \
        exit 1; \
    fi && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf aws awscliv2.zip

# Create a directory for CloudPEASS
WORKDIR /opt/cloudpeass

# Clone the CloudPEASS repository
RUN git clone https://github.com/carlospolop/CloudPEASS.git . && \
    pip install --no-cache-dir boto3 botocore requests openai tqdm colorama

# Define environment variables for AWS credentials
# These can be overridden when running the container
ENV AWS_PROFILE=default
ENV AWS_REGION=us-east-1
ENV AWS_CONFIG_FILE=/root/.aws/config
ENV AWS_SHARED_CREDENTIALS_FILE=/root/.aws/credentials

# Create directories for AWS credentials
RUN mkdir -p /root/.aws

# Create an entrypoint script to handle AWS credentials and launch AWSPEAS
RUN echo '#!/bin/bash\n\
if [ -n "$AWS_ACCESS_KEY_ID" ] && [ -n "$AWS_SECRET_ACCESS_KEY" ]; then\n\
  mkdir -p /root/.aws\n\
  echo "[default]" > /root/.aws/credentials\n\
  echo "aws_access_key_id = $AWS_ACCESS_KEY_ID" >> /root/.aws/credentials\n\
  echo "aws_secret_access_key = $AWS_SECRET_ACCESS_KEY" >> /root/.aws/credentials\n\
  if [ -n "$AWS_SESSION_TOKEN" ]; then\n\
    echo "aws_session_token = $AWS_SESSION_TOKEN" >> /root/.aws/credentials\n\
  fi\n\
  echo "[default]" > /root/.aws/config\n\
  echo "region = $AWS_REGION" >> /root/.aws/config\n\
  echo "output = json" >> /root/.aws/config\n\
fi\n\
\n\
# Construct the AWSPEAS command with provided arguments\n\
CMD="python3 AWSPEAS.py --profile $AWS_PROFILE --region $AWS_REGION"\n\
\n\
# Add additional arguments if provided\n\
if [ -n "$AWSPEAS_OUT_JSON_PATH" ]; then\n\
  CMD="$CMD --out-json-path $AWSPEAS_OUT_JSON_PATH"\n\
fi\n\
\n\
if [ -n "$AWSPEAS_THREADS" ]; then\n\
  CMD="$CMD --threads $AWSPEAS_THREADS"\n\
fi\n\
\n\
if [ "$AWSPEAS_NOT_USE_HACKTRICKS_AI" = "true" ]; then\n\
  CMD="$CMD --not-use-hacktricks-ai"\n\
fi\n\
\n\
if [ "$AWSPEAS_DEBUG" = "true" ]; then\n\
  CMD="$CMD --debug"\n\
fi\n\
\n\
if [ -n "$AWSPEAS_AWS_SERVICES" ]; then\n\
  CMD="$CMD --aws-services $AWSPEAS_AWS_SERVICES"\n\
fi\n\
\n\
# Execute the command\n\
echo "Executing: $CMD"\n\
exec $CMD\n'\
> /opt/cloudpeass/entrypoint.sh && \
chmod +x /opt/cloudpeass/entrypoint.sh

# Set the entrypoint
ENTRYPOINT ["/opt/cloudpeass/entrypoint.sh"]
