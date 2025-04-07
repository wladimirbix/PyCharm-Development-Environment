FROM python:3.12

# Install necessary packages
RUN curl -fsSL https://raw.githubusercontent.com/databricks/setup-cli/main/install.sh | sh

RUN apt-get update -y && \
    apt-get install -y azure-cli

RUN apt-get update -y && \
    apt-get install -y git

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Copy the requirements.txt 
COPY requirements.txt requirements.txt

# Create virtual environment and install dependencies
RUN uv venv --python '>=3.12' /.venv && \
    . /.venv/bin/activate && \
    uv pip install -r requirements.txt

# Ensure the virtual environment is used and recognized by Databricks/VSCode
ENV PATH="/.venv/bin:$PATH"
ENV VIRTUAL_ENV="/.venv"

# Copy the project files
WORKDIR /project
COPY ./ .

# Create a script to automatically activate the virtual environment on container start
RUN echo '#!/bin/bash\nsource /.venv/bin/activate\nexec "$@"' > /entrypoint.sh && \
    chmod +x /entrypoint.sh

# Set the entrypoint to automatically activate the venv when the container starts
ENTRYPOINT ["/entrypoint.sh"]

# Add alias for 'unicon' globally
RUN echo 'unicon() { python3 "$(find /workspaces -maxdepth 4 -type f -name cli.py 2>/dev/null)"; }' >> /etc/bash.bashrc