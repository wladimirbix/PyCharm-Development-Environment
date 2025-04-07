FROM python:3.12

# Install necessary packages
RUN curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sh

RUN apt-get update -y && \
    apt-get install -y azure-cli

RUN apt-get update -y && \
    apt-get install -y git

# Copy the requirements.txt 
COPY requirements.txt requirements.txt

# Copy the project files
WORKDIR /project
COPY ./ .

# Add alias for 'unicon' globally
RUN echo 'unicon() { python3 "$(find /workspaces -maxdepth 4 -type f -name cli.py 2>/dev/null)"; }' >> /etc/bash.bashrc