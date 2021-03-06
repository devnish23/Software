Install Compose on Linux systems
On Linux, you can download the Docker Compose binary from the Compose repository release page on GitHub. Follow the instructions from the link, which involve running the curl command in your terminal to download the binaries. These step-by-step instructions are also included below.

For alpine, the following dependency packages are needed: py-pip, python3-dev, libffi-dev, openssl-dev, gcc, libc-dev, rust, cargo and make.

Run this command to download the current stable release of Docker Compose:

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
To install a different version of Compose, substitute 1.29.2 with the version of Compose you want to use.

If you have problems installing with curl, see Alternative Install Options tab above.

Apply executable permissions to the binary:

sudo chmod +x /usr/local/bin/docker-compose
Note: If the command docker-compose fails after installation, check your path. You can also create a symbolic link to /usr/bin or any other directory in your path.

For example:

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
Optionally, install command completion for the bash and zsh shell.

Test the installation.

$ docker-compose --version
docker-compose version 1.29.2, build 1110ad01

