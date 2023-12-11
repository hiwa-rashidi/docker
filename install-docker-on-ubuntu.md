# Install Docker Engine on Ubuntu


1. [Prerequisites](#prerequisites) 

    1-1. OS requirements

    1-2. Checking the Firewall

    1-3. Uninstall old versions 

2. [Install Docker Engine](#install-docker-engine)

    2-1. Installation methods 

    2-2. Install Docker from a package 

    2-3. Configure Docker to start on boot 

3. [Install Docker Compose](#install-docker-compose)

    3-1. Install Compose Plugin

    3-2. Install Compose Standalone

4. [Unistall Docker Engine](#unistall-docker-engine)

5. [References](#references)


## Prerequisites

### 1-1. OS requirements

To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

* Ubuntu Lunar 23.04
* Ubuntu Kinetic 22.10
* Ubuntu Jammy 22.04 (LTS)
* Ubuntu Focal 20.04 (LTS)

Docker Engine for Ubuntu is compatible with x86_64 (or amd64), armhf, arm64, s390x,
and ppc64le (ppc64el) architectures.

You can check the OS release using the below command:

```
$ cat /etc/os-release
```

and to check the OS architecture, run the following command: 

```
$ dpkg --print-architecture
```

### 1-2. Checking the Firewall

If you use ufw or firewalld to manage firewall settings, be aware that when you
expose container ports using Docker, these ports bypass your firewall rules. For
more information, refer to [Docker and ufw](https://docs.docker.com/network/packet-filtering-firewalls/#docker-and-ufw).

Check the ufw status by the following command:

```
$ sudo ufw status
```

### 1-3. Uninstall old versions

Before you can install Docker Engine, you must first make sure that any conflicting
packages are uninstalled.

The unofficial packages to uninstall are:

* `docker.io`
* `docker-compose`
* `docker-doc`
* `podman-docker`


Moreover, Docker Engine depends on `containerd` and `runc`. Docker Engine
bundles these dependencies as one bundle: `containerd.io`. If you have installed the
containerd or runc previously, uninstall them to avoid conflicts with the versions
bundled with Docker Engine.

Run the following command to uninstall all conflicting packages:

```
$ for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

apt-get might report that you have none of these packages installed.

Images, containers, volumes, and networks stored in `/var/lib/docker/` aren’t
automatically removed when you uninstall Docker. If you want to start with a clean
installation, and prefer to clean up any existing data, read the [Uninstall Docker Engine](#uninstall-docker-engine)
section.


## Install Docker Engine 


### 2-1. Installation methods

You can install Docker Engine in different ways, depending on your needs:

* Docker Engine comes bundled with [Docker Desktop for Linux](https://docs.docker.com/desktop/install/linux-install/). This is the easiest and quickest way to get started.

* Set up and install Docker Engine from [Docker’s apt repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository).

* [Install it manually](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) and manage upgrades manually.

* Use a [convenience script](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script). Only recommended for testing and development environments.


#### 2-2. Install Docker from a package

If you can’t use Docker’s apt repository to install Docker Engine, you can download
the deb file for your release and install it manually. You need to download a new file
each time you want to upgrade Docker Engine.


1. Go to https://download.docker.com/linux/ and select the OS distribution.

![Docker Installation Img-01](./docker-installation-imgages/docker-installation-img-01.jpg)


2. Select the dists and select the distribution release (or version code name).

![Docker Installation Img-02](./docker-installation-imgages/docker-installation-img-02.jpg)

![Docker Installation Img-03](./docker-installation-imgages/docker-installation-img-03.jpg)


3. Go to pool/stable/ and select the applicable architecture (amd64, armhf, arm64, or s390x).

![Docker Installation Img-04](./docker-installation-imgages/docker-installation-img-04.jpg)

![Docker Installation Img-05](./docker-installation-imgages/docker-installation-img-05.jpg)

![Docker Installation Img-06](./docker-installation-imgages/docker-installation-img-06.jpg)


4. Download the following deb files for the Docker Engine, CLI, containerd, and
Docker Compose packages:

 - containerd.io_\<version\>_\<arch\>.deb
 - docker-ce_\<version\>_\<arch\>.deb
 - docker-ce-cli_\<version\>_\<arch\>.deb
 - docker-buildx-plugin_\<version\>_\<arch\>.deb
 - docker-compose-plugin_\<version\>_\<arch\>.deb


Also yo can the copy link address of packages (by right click on the each package name) and download
them on the server with the internet access using the wget command.

```
$ mkdir /your/path/docker.packages

$ cd /your/path/docker.packages 

$ wget https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/containerd.io_1.6.9-1_amd64.deb

$ wget https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-ce_24.0.5-1\~ubuntu.22.04\~jammy_amd64.deb

$ wget https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-ce-cli_24.0.5-1\~ubuntu.22.04\~jammy_amd64.deb

$ wget https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-buildx-plugin_0.11.2-1\~ubuntu.22.04\~jammy_amd64.deb

$ wget https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-compose-plugin_2.6.0\~ubuntu-jammy_amd64.deb
```


5. Install the .deb packages. Update the paths in the following example to where
you downloaded the Docker packages.

```
$ cd /your/path/docker.packages 

$ dpkg -i *.deb  

Output:

Selecting previously unselected package containerd.io.
(Reading database ... 73915 files and directories currently installed.)
Preparing to unpack containerd.io_1.6.9-1_amd64.deb ...
Unpacking containerd.io (1.6.9-1) ...
Selecting previously unselected package docker-buildx-plugin.
Preparing to unpack docker-buildx-plugin_0.11.2-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-buildx-plugin (0.11.2-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-ce.
Preparing to unpack docker-ce_24.0.5-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-ce (5:24.0.5-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-ce-cli.
Preparing to unpack docker-ce-cli_24.0.5-1~ubuntu.22.04~jammy_amd64.deb ...
Unpacking docker-ce-cli (5:24.0.5-1~ubuntu.22.04~jammy) ...
Selecting previously unselected package docker-compose-plugin.
Preparing to unpack docker-compose-plugin_2.6.0~ubuntu-jammy_amd64.deb ...
Unpacking docker-compose-plugin (2.6.0~ubuntu-jammy) ...
Setting up containerd.io (1.6.9-1) ...
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /lib/systemd/system/containerd.service.
Setting up docker-ce-cli (5:24.0.5-1~ubuntu.22.04~jammy) ...
Setting up docker-compose-plugin (2.6.0~ubuntu-jammy) ...
Setting up docker-ce (5:24.0.5-1~ubuntu.22.04~jammy) ...
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
Processing triggers for man-db (2.10.2-1) ...
```

As you can see, the systemd created a symbolik link to the docker and containerd services. It's useful to start 
automatically the docker engine on system boot.


6. Check the service status: 

```
$ systemctl status docker

Output:

● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-09-13 17:54:07 UTC; 3min 8s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 7437 (dockerd)
      Tasks: 10
     Memory: 26.7M
        CPU: 1.016s
     CGroup: /system.slice/docker.service
             └─7437 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 13 17:54:05 son.test.30.110 systemd[1]: Starting Docker Application Container Engine...
Sep 13 17:54:06 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:06.045846941Z" level=info msg="Starting up"
Sep 13 17:54:06 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:06.048836006Z" level=info msg="detected 127.0.0.53 nameserver>
Sep 13 17:54:06 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:06.303455543Z" level=info msg="Loading containers: start."
Sep 13 17:54:07 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:07.758698741Z" level=info msg="Loading containers: done."
Sep 13 17:54:07 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:07.803579087Z" level=info msg="Docker daemon" commit=a61e2b4 >
Sep 13 17:54:07 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:07.804126864Z" level=info msg="Daemon has completed initializ>
Sep 13 17:54:07 son.test.30.110 dockerd[7437]: time="2022-09-13T17:54:07.907631258Z" level=info msg="API listen on /run/docker.sock"
Sep 13 17:54:07 son.test.30.110 systemd[1]: Started Docker Application Container Engine.
```

### 2-3. Configure Docker to start on boot

Check the docker service is enabled. On Debian and Ubuntu, the Docker service starts on boot by default.

```
$ systemctl is-enabled docker.service
enabled


$ systemctl is-enabled containerd.service
enabled
``` 

To automatically start Docker and containerd on boot for other Linux distributions using systemd, run the following commands:
```
$ sudo systemctl enable docker.service
$ sudo systemctl enable containerd.service
```

## Install Docker Compose

### 3-1. Install Compose Plugin

We installed the docker-compose-plugin in the previous section (2-2. Install Docker from a package). 
Verify that Docker Compose is installed correctly by checking the version.

```
$ docker compose version
Docker Compose version v2.6.0
```


### 3-2. Install Compose Standalone

Note that Compose standalone uses the -compose syntax instead of the current standard syntax compose.
For example type `docker-compose up` when using Compose standalone, instead of `docker compose up`.


1. To download and install Compose standalone, run:

```
$ curl -SL https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

2. Apply executable permissions to the standalone binary in the target path for the installation.

```
$ sudo chmod +x /usr/local/bin/docker-compose
```

3. Test and execute compose commands using docker-compose. 

```
$ docker-compose --version
```



In another way, you can use the docker-compose file in the cli-plugin, instead of downloading using curl
or browser. 


```
$ docker-compose --version
-bash: /usr/local/bin/docker-compose: No such file or directory


$ sudo find / -type d -name cli-plugins
/usr/libexec/docker/cli-plugins


$ ls -lh /usr/libexec/docker/cli-plugins
total 100M
-rwxr-xr-x 1 root root 75M Jul 21 20:35 docker-buildx
-rwxr-xr-x 1 root root 26M Jun  6  2022 docker-compose


$ ls -lh /usr/libexec/docker/cli-plugins/docker-compose
-rwxr-xr-x 1 root root 26M Jun  6  2022 /usr/libexec/docker/cli-plugins/docker-compose
```


You can copy the `/usr/libexec/docker/cli-plugins/docker-compose` file to the `/usr/local/bin/docker-compose` path
and then set the permission to executable.

```
$ ls -lh /usr/local/bin
total 0

$ sudo cp /usr/libexec/docker/cli-plugins/docker-compose /usr/local/bin/


$ ls -lh /usr/local/bin
total 26M
-rwxr-xr-x 1 root root 26M Aug  1 12:32 docker-compose


$ stat -c '%A %a' /usr/local/bin/docker-compose
-rwxr-xr-x 755


$ docker-compose --version
Docker Compose version v2.6.0
```


## Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:

```
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

2. Images, containers, volumes, or custom configuration files on your host aren’t
automatically removed. To delete all images, containers, and volumes:

```
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

You have to delete any edited configuration files manually.


## References


1. [Docker Engine overview](https://docs.docker.com/engine/)

2. [Install Docker Engine](https://docs.docker.com/engine/install/)

3. [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

4. [Linux post-installation steps for Docker Engine](https://docs.docker.com/engine/install/linux-postinstall/)

5. [Install the Compose plugin](https://docs.docker.com/compose/install/linux/)

6. [Install Compose standalone](https://docs.docker.com/compose/install/standalone/) 
