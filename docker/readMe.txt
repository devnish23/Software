Install Docker Engine on CentOS
=================================
Estimated reading time: 11 minutes

To get started with Docker Engine on CentOS, make sure you meet the prerequisites, then install Docker.

Prerequisites
OS requirements
To install Docker Engine, you need a maintained version of CentOS 7 or 8. Archived versions aren’t supported or tested.

The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it.

The overlay2 storage driver is recommended.

Uninstall old versions
Older versions of Docker were called docker or docker-engine. If these are installed, uninstall them, along with associated dependencies.

 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
It’s OK if yum reports that none of these packages are installed.

The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. The Docker Engine package is now called docker-ce.

Installation methods
You can install Docker Engine in different ways, depending on your needs:

Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.

Some users download the RPM package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.

In testing and development environments, some users choose to use automated convenience scripts to install Docker.

Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

Set up the repository
Install the yum-utils package (which provides the yum-config-manager utility) and set up the stable repository.

 sudo yum install -y yum-utils
 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
Optional: Enable the nightly or test repositories.

These repositories are included in the docker.repo file above but are disabled by default. You can enable them alongside the stable repository. The following command enables the nightly repository.

 sudo yum-config-manager --enable docker-ce-nightly
To enable the test channel, run the following command:

 sudo yum-config-manager --enable docker-ce-test
You can disable the nightly or test repository by running the yum-config-manager command with the --disable flag. To re-enable it, use the --enable flag. The following command disables the nightly repository.

 sudo yum-config-manager --disable docker-ce-nightly
Learn about nightly and test channels.

Install Docker Engine
Install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

 sudo yum install docker-ce docker-ce-cli containerd.io
If prompted to accept the GPG key, verify that the fingerprint matches 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the yum install or yum update command always installs the highest possible version, which may not be appropriate for your stability needs.

Docker is installed but not started. The docker group is created, but no users are added to the group.

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List and sort the versions available in your repo. This example sorts results by version number, highest to lowest, and is truncated:

 yum list docker-ce --showduplicates | sort -r
The list returned depends on which repositories are enabled, and is specific to your version of CentOS (indicated by the .el7 suffix in this example).

b. Install a specific version by its fully qualified package name, which is the package name (docker-ce) plus the version string (2nd column) starting at the first colon (:), up to the first hyphen, separated by a hyphen (-). For example, docker-ce-18.09.1.

 sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
Docker is installed but not started. The docker group is created, but no users are added to the group.

Start Docker.

 sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

 sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

Docker Engine is installed and running. You need to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.

Upgrade Docker Engine
To upgrade Docker Engine, follow the installation instructions, choosing the new version you want to install.

Install from a package
If you cannot use Docker’s repository to install Docker, you can download the .rpm file for your release and install it manually. You need to download a new file each time you want to upgrade Docker Engine.

Go to https://download.docker.com/linux/centos/ and choose your version of CentOS. Then browse to x86_64/stable/Packages/ and download the .rpm file for the Docker version you want to install.

Note: To install a nightly or test (pre-release) package, change the word stable in the above URL to nightly or test. Learn about nightly and test channels.

Install Docker Engine, changing the path below to the path where you downloaded the Docker package.

 sudo yum install /path/to/package.rpm
Docker is installed but not started. The docker group is created, but no users are added to the group.

Start Docker.

 sudo systemctl start docker
Verify that Docker Engine is installed correctly by running the hello-world image.

 sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

Docker Engine is installed and running. You need to use sudo to run Docker commands. Continue to Post-installation steps for Linux to allow non-privileged users to run Docker commands and for other optional configuration steps.

Upgrade Docker Engine
To upgrade Docker Engine, download the newer package file and repeat the installation procedure, using yum -y upgrade instead of yum -y install, and pointing to the new file.

Install using the convenience script
Docker provides a convenience script at get.docker.com to install Docker into development environments quickly and non-interactively. The convenience script is not recommended for production environments, but can be used as an example to create a provisioning script that is tailored to your needs. Also refer to the install using the repository steps to learn about installation steps to install using the package repository. The source code for the script is open source, and can be found in the docker-install repository on GitHub.

Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script:

The script requires root or sudo privileges to run.
The script attempts to detect your Linux distribution and version and configure your package management system for you, and does not allow you to customize most installation parameters.
The script installs dependencies and recommendations without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
By default, the script installs the latest stable release of Docker, containerd, and runc. When using this script to provision a machine, this may result in unexpected major version upgrades of Docker. Always test (major) upgrades in a test environment before deploying to your production systems.
The script is not designed to upgrade an existing Docker installation. When using the script to update an existing installation, dependencies may not be updated to the expected version, causing outdated versions to be used.
Tip: preview script steps before running

You can run the script with the DRY_RUN=1 option to learn what steps the script will execute during installation:

 curl -fsSL https://get.docker.com -o get-docker.sh
 DRY_RUN=1 sh ./get-docker.sh
This example downloads the script from get.docker.com and runs it to install the latest stable release of Docker on Linux:

 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
Docker is installed. The docker service starts automatically on Debian based distributions. On RPM based distributions, such as CentOS or Fedora, you need to start it manually using the appropriate systemctl or service command. As the message indicates, non-root users cannot run Docker commands by default.

Use Docker as a non-privileged user, or install in rootless mode?

The installation script requires root or sudo privileges to install and use Docker. If you want to grant non-root users access to Docker, refer to the post-installation steps for Linux. Docker can also be installed without root privileges, or configured to run in rootless mode. For instructions on running Docker in rootless mode, refer to run the Docker daemon as a non-root user (rootless mode).

Install pre-releases
Docker also provides a convenience script at test.docker.com to install pre-releases of Docker on Linux. This script is equivalent to the script at get.docker.com, but configures your package manager to enable the “test” channel from our package repository, which includes both stable and pre-releases (beta versions, release-candidates) of Docker. Use this script to get early access to new releases, and to evaluate them in a testing environment before they are released as stable.

To install the latest version of Docker on Linux from the “test” channel, run:

 curl -fsSL https://test.docker.com -o test-docker.sh
 sudo sh test-docker.sh
Upgrade Docker after using the convenience script
If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There is no advantage to re-running the convenience script, and it can cause issues if it attempts to re-add repositories which have already been added to the host machine.

Uninstall Docker Engine
Uninstall the Docker Engine, CLI, and Containerd packages:

 sudo yum remove docker-ce docker-ce-cli containerd.io
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

 sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
You must delete any edited configuration files manually.

===END Installation===


Post-installation steps for Linux
Estimated reading time: 15 minutes

This section contains optional procedures for configuring Linux hosts to work better with Docker.

Manage Docker as a non-root user
The Docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The Docker daemon always runs as the root user.

If you don’t want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

Warning

The docker group grants privileges equivalent to the root user. For details on how this impacts security in your system, see Docker Daemon Attack Surface.

Note:

To run Docker without root privileges, see Run the Docker daemon as a non-root user (Rootless mode).

To create the docker group and add your user:

Create the docker group.

 sudo groupadd docker
Add your user to the docker group.

 sudo usermod -aG docker $USER
Log out and log back in so that your group membership is re-evaluated.

If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.

On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.

On Linux, you can also run the following command to activate the changes to groups:

 newgrp docker 
Verify that you can run docker commands without sudo.

 docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

If you initially ran Docker CLI commands using sudo before adding your user to the docker group, you may see the following error, which indicates that your ~/.docker/ directory was created with incorrect permissions due to the sudo commands.

WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
To fix this problem, either remove the ~/.docker/ directory (it is recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:

 sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
 sudo chmod g+rwx "$HOME/.docker" -R
Configure Docker to start on boot
Most current Linux distributions (RHEL, CentOS, Fedora, Debian, Ubuntu 16.04 and higher) use systemd to manage which services start when the system boots. On Debian and Ubuntu, the Docker service is configured to start on boot by default. To automatically start Docker and Containerd on boot for other distros, use the commands below:

 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
To disable this behavior, use disable instead.

 sudo systemctl disable docker.service
 sudo systemctl disable containerd.service
If you need to add an HTTP Proxy, set a different directory or partition for the Docker runtime files, or make other customizations, see customize your systemd Docker daemon options.

Use a different storage engine
For information about the different storage engines, see Storage drivers. The default storage engine and the list of supported storage engines depend on your host’s Linux distribution and available kernel drivers.

Configure default logging driver
Docker provides the capability to collect and view log data from all containers running on a host via a series of logging drivers. The default logging driver, json-file, writes log data to JSON-formatted files on the host filesystem. Over time, these log files expand in size, leading to potential exhaustion of disk resources.

To alleviate such issues, either configure the json-file logging driver to enable log rotation, use an alternative logging driver such as the “local” logging driver that performs log rotation by default, or use a logging driver that sends logs to a remote logging aggregator.

Configure where the Docker daemon listens for connections
By default, the Docker daemon listens for connections on a UNIX socket to accept requests from local clients. It is possible to allow Docker to accept requests from remote hosts by configuring it to listen on an IP address and port as well as the UNIX socket. For more detailed information on this configuration option take a look at “Bind Docker to another host/port or a unix socket” section of the Docker CLI Reference article.

Secure your connection

Before configuring Docker to accept connections from remote hosts it is critically important that you understand the security implications of opening docker to the network. If steps are not taken to secure the connection, it is possible for remote non-root users to gain root access on the host. For more information on how to use TLS certificates to secure this connection, check this article on how to protect the Docker daemon socket.

Configuring Docker to accept remote connections can be done with the docker.service systemd unit file for Linux distributions using systemd, such as recent versions of RedHat, CentOS, Ubuntu and SLES, or with the daemon.json file which is recommended for Linux distributions that do not use systemd.

systemd vs daemon.json

Configuring Docker to listen for connections using both the systemd unit file and the daemon.json file causes a conflict that prevents Docker from starting.

Configuring remote access with systemd unit file
Use the command sudo systemctl edit docker.service to open an override file for docker.service in a text editor.

Add or modify the following lines, substituting your own values.

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
Save the file.

Reload the systemctl configuration.

 sudo systemctl daemon-reload
Restart Docker.

 sudo systemctl restart docker.service
Check to see whether the change was honored by reviewing the output of netstat to confirm dockerd is listening on the configured port.

 sudo netstat -lntp | grep dockerd
Configuring remote access with daemon.json
Set the hosts array in the /etc/docker/daemon.json to connect to the UNIX socket and an IP address, as follows:

{
  "hosts": ["unix:///var/run/docker.sock", "tcp://127.0.0.1:2375"]
}
Restart Docker.

Check to see whether the change was honored by reviewing the output of netstat to confirm dockerd is listening on the configured port.

 sudo netstat -lntp | grep dockerd
Enable IPv6 on the Docker daemon
To enable IPv6 on the Docker daemon, see Enable IPv6 support.

Troubleshooting
Kernel compatibility
Docker cannot run correctly if your kernel is older than version 3.10 or if it is missing some modules. To check kernel compatibility, you can download and run the check-config.sh script.

 curl https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh > check-config.sh
 bash ./check-config.sh
The script only works on Linux, not macOS.

Cannot connect to the Docker daemon
If you see an error such as the following, your Docker client may be configured to connect to a Docker daemon on a different host, and that host may not be reachable.

Cannot connect to the Docker daemon. Is 'docker daemon' running on this host?
To see which host your client is configured to connect to, check the value of the DOCKER_HOST variable in your environment.

 env | grep DOCKER_HOST
If this command returns a value, the Docker client is set to connect to a Docker daemon running on that host. If it is unset, the Docker client is set to connect to the Docker daemon running on the local host. If it is set in error, use the following command to unset it:

 unset DOCKER_HOST
You may need to edit your environment in files such as ~/.bashrc or ~/.profile to prevent the DOCKER_HOST variable from being set erroneously.

If DOCKER_HOST is set as intended, verify that the Docker daemon is running on the remote host and that a firewall or network outage is not preventing you from connecting.

IP forwarding problems
If you manually configure your network using systemd-network with systemd version 219 or higher, Docker containers may not be able to access your network. Beginning with systemd version 220, the forwarding setting for a given network (net.ipv4.conf.<interface>.forwarding) defaults to off. This setting prevents IP forwarding. It also conflicts with Docker’s behavior of enabling the net.ipv4.conf.all.forwarding setting within containers.

To work around this on RHEL, CentOS, or Fedora, edit the <interface>.network file in /usr/lib/systemd/network/ on your Docker host (ex: /usr/lib/systemd/network/80-container-host0.network) and add the following block within the [Network] section.

[Network]
...
IPForward=kernel
# OR
IPForward=true
This configuration allows IP forwarding from the container as expected.

DNS resolver found in resolv.conf and containers can't use it
Linux systems which use a GUI often have a network manager running, which uses a dnsmasq instance running on a loopback address such as 127.0.0.1 or 127.0.1.1 to cache DNS requests, and adds this entry to /etc/resolv.conf. The dnsmasq service speeds up DNS look-ups and also provides DHCP services. This configuration does not work within a Docker container which has its own network namespace, because the Docker container resolves loopback addresses such as 127.0.0.1 to itself, and it is very unlikely to be running a DNS server on its own loopback address.

If Docker detects that no DNS server referenced in /etc/resolv.conf is a fully functional DNS server, the following warning occurs and Docker uses the public DNS servers provided by Google at 8.8.8.8 and 8.8.4.4 for DNS resolution.

WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers
can't use it. Using default external servers : [8.8.8.8 8.8.4.4]
If you see this warning, first check to see if you use dnsmasq:

 ps aux |grep dnsmasq
If your container needs to resolve hosts which are internal to your network, the public nameservers are not adequate. You have two choices:

You can specify a DNS server for Docker to use, or
You can disable dnsmasq in NetworkManager. If you do this, NetworkManager adds your true DNS nameserver to /etc/resolv.conf, but you lose the possible benefits of dnsmasq.
You only need to use one of these methods.

Specify DNS servers for Docker
The default location of the configuration file is /etc/docker/daemon.json. You can change the location of the configuration file using the --config-file daemon flag. The documentation below assumes the configuration file is located at /etc/docker/daemon.json.

Create or edit the Docker daemon configuration file, which defaults to /etc/docker/daemon.json file, which controls the Docker daemon configuration.

 sudo nano /etc/docker/daemon.json
Add a dns key with one or more IP addresses as values. If the file has existing contents, you only need to add or edit the dns line.

{
  "dns": ["8.8.8.8", "8.8.4.4"]
}
If your internal DNS server cannot resolve public IP addresses, include at least one DNS server which can, so that you can connect to Docker Hub and so that your containers can resolve internet domain names.

Save and close the file.

Restart the Docker daemon.

 sudo service docker restart
Verify that Docker can resolve external IP addresses by trying to pull an image:

 docker pull hello-world
If necessary, verify that Docker containers can resolve an internal hostname by pinging it.

 docker run --rm -it alpine ping -c4 <my_internal_host>
Disable dnsmasq
Ubuntu
If you prefer not to change the Docker daemon’s configuration to use a specific IP address, follow these instructions to disable dnsmasq in NetworkManager.

Edit the /etc/NetworkManager/NetworkManager.conf file.

Comment out the dns=dnsmasq line by adding a # character to the beginning of the line.

# dns=dnsmasq
Save and close the file.

Restart both NetworkManager and Docker. As an alternative, you can reboot your system.

 sudo systemctl restart network-manager
 sudo systemctl restart docker
RHEL, CentOS, or Fedora
To disable dnsmasq on RHEL, CentOS, or Fedora:

Disable the dnsmasq service:

 sudo systemctl stop dnsmasq
 sudo systemctl disable dnsmasq
Configure the DNS servers manually using the Red Hat documentation.

Allow access to the remote API through a firewall
If you run a firewall on the same host as you run Docker and you want to access the Docker Remote API from another host and remote access is enabled, you need to configure your firewall to allow incoming connections on the Docker port, which defaults to 2376 if TLS encrypted transport is enabled or 2375 otherwise.

Two common firewall daemons are UFW (Uncomplicated Firewall) (often used for Ubuntu systems) and firewalld (often used for RPM-based systems). Consult the documentation for your OS and firewall, but the following information might help you get started. These options are fairly permissive and you may want to use a different configuration that locks your system down more.

UFW: Set DEFAULT_FORWARD_POLICY="ACCEPT" in your configuration.

firewalld: Add rules similar to the following to your policy (one for incoming requests and one for outgoing requests). Be sure the interface names and chain names are correct.

<direct>
  [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -i zt0 -j ACCEPT </rule> ]
  [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -o zt0 -j ACCEPT </rule> ]
</direct>
Your kernel does not support cgroup swap limit capabilities
On Ubuntu or Debian hosts, You may see messages similar to the following when working with an image.

WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.
This warning does not occur on RPM-based systems, which enable these capabilities by default.

If you don’t need these capabilities, you can ignore the warning. You can enable these capabilities on Ubuntu or Debian by following these instructions. Memory and swap accounting incur an overhead of about 1% of the total available memory and a 10% overall performance degradation, even if Docker is not running.

Log into the Ubuntu or Debian host as a user with sudo privileges.

Edit the /etc/default/grub file. Add or edit the GRUB_CMDLINE_LINUX line to add the following two key-value pairs:

GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
Save and close the file.

Update GRUB.

 sudo update-grub
If your GRUB configuration file has incorrect syntax, an error occurs. In this case, repeat steps 2 and 3.

The changes take effect when the system is rebooted.

==END of Post config==
