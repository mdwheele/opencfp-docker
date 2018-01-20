# Dockerized OpenCFP (beta)

This repository is meant to be a Docker-based development environment for OpenCFP.

## Manual

```bash
# Preparations
# ============
# 
# Go into your clone of OpenCFP
cd /path/to/opencfp
#
# Clone this project (or your fork of it) into a folder .docker
git clone https://github.com/mdwheele/opencfp-docker.git .docker
#
# Add the new folder to your repos ignored folder list
echo .docker >> .git/info/exclude
#
# and then move into the .docker folder to go on
cd .docker

# Usage
# =====
#
# You'll use the `cfp` shell to interact with the Docker 
# environment
./cfp
# > Usage: ./cfp {build|up|down|shell|clean}

# On the first run, build the `workspace` image we'll work from.
# It takes a little bit to build, but this is only done once.
# The workspace is based on PHP 7.1 + PHP-FPM.
# It installs things like Composer, NodeJS, npm, yarn, etc.
./cfp build

# This does NOT deploy the application. That is YOUR job after 
# all containers have started. You'll need a shell.
# To get a shell to the `workspace` container...
./cfp shell

# The `shell` command starts all the containers (runs `up`) and starts bash.
# You'll be dropped in `/usr/src/app` where the project is bind-mounted to
# by Docker. In this way, you can make changes to the project on your 
# host and they will be automatically available in the container. Be sure
# to pay attention to where you're running commands. 

# Go through the normal setup steps for OpenCFP...

# Configure the environment
cp config/development.dist.yml config/development.yml
vi config/development.yml

# Install dependencies and migrate database
script/setup

# When you're done, you can exit and stop all containers.

# Leave the bash session on the workspace container...
exit 

# Stop all containers...
./cfp down

# Alternatively, if you're done and want containers removed...
./cfp clean

# At this point I would also clean up unused images and volumes...
docker system prune -a
```
