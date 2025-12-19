# Jenkins Agent Docker Compose

This repository contains a Docker Compose configuration to run a Jenkins inbound agent configured for Yocto and QNX development.

## Prerequisites

- Docker installed on your system
- Docker Compose installed
- Access to a Jenkins master server
- Yocto and QNX development directories set up on the host (see Volumes section)

## Environment Variables

Before running the container, you need to set the following environment variables:

- `JENKINS_URL`: The URL of your Jenkins master server (e.g., `http://jenkins.example.com:8080`)
- `JENKINS_AGENT_NAME`: The name of the agent as configured in Jenkins
- `JENKINS_SECRET`: The secret token provided by Jenkins for this agent

You can set these variables in a `.env` file in the same directory as `docker-compose.yml`, or export them in your shell.

Example `.env` file:

```sh
JENKINS_URL=http://your-jenkins-server:8080
JENKINS_AGENT_NAME=my-agent
JENKINS_SECRET=your-secret-token-here
```

## Usage

1. Clone or download this repository
2. Ensure the required host directories exist (see Volumes section)
3. Set up the environment variables as described above
4. Run the following command to start the Jenkins agent:

```bash
docker-compose up
```

The agent will connect to the Jenkins master using WebSocket and be available for running Yocto and QNX build jobs.

## Volumes

The following host directories are mounted into the container for development purposes:

- `~/.yocto/cache/downloads:/home/jenkins/.yocto/cache/downloads` - Yocto downloads cache
- `~/.yocto/cache/sstate:/home/jenkins/.yocto/cache/sstate` - Yocto shared state cache
- `~/qnx800/:/home/jenkins/qnx800` - QNX 8.0 installation directory
- `~/.qnx:/home/jenkins/.qnx` - QNX configuration and cache directory

Ensure these directories exist on your host system before running the container. You may need to create them if they don't exist:

```bash
mkdir -p ~/.yocto/cache/downloads ~/.yocto/cache/sstate ~/qnx800 ~/.qnx
```

## Notes

- The container runs as user `1000:1000` to avoid permission issues with mounted volumes
- The service is configured to restart automatically unless stopped manually
- This setup is optimized for CI/CD pipelines involving Yocto builds and QNX development