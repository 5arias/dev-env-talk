# Development Environments
* Why use dev environments other than your machine's OS?
    - Your application (probably) won't be deployed on Windows/OSX.
    - Your hardware does not match the server's.
    - To be certain of performance and **configuration** we develop on environments
    as close to the deployed environment as possible.
* Tools your app probably is depolyed with (not mutually exclusive):
    - Amazon Web Services (AWS)
    - Docker / Docker Compose
    - Vagrant/Packer (configured system images)
* Tools for configuration
    - To make sure all envs are configured the same way, you might be using some of these tools:
        - Chef => Ruby program to
        - Puppet
        - Docker
        - custom scripting!

## Vagrant with VirtualBox
Vagrant uses other virtual machine providers (like VirtualBox, Parallels, or AWS) to create customized virtual machines.
Can configure OS, # CPUs, Memory, Disk, shared directories (with your local machine), open/closed ports, custom IPs, etc.
* Install VirtualBox => https://www.virtualbox.org/wiki/Downloads
* Install Vagrant => https://www.vagrantup.com/downloads.html
* `$ mkdir my-dev-env`
* `$ cd my-dev-env`
* `$ vagrant init`
* build your Vagrantfile (see example)
* `$ vagrant up` and wait... it is probably downloading Ubuntu...
* `$ vagrant ssh` and you're in.

* Basic Vagrant commands:
    -
    - `vagrant init` #=> creates a Vagrantfile in your directory,
    - `vagrant up` #=> starts your VM
    - `vagrant ssh` #=> connects to your VM via SSH. Can have many connections.
    - `exit` #=> exits SSH session, back to local machine.
    - `vagrant halt` #=> triggers VM shutdown. Must be done from outside VM.
    - `vagrant destroy` #=> removes VM (!)

## Docker
### What is Docker?
- Docker is a tool for layering application resources in a modular fashion to minimize disk space and improve your app's portability.
- Docker is NOT a VM, it is a collection of filesystem images layerd on top of each other and sharing resources. The base Docker image is a barebones Linux kernel. Everything else is added as needed.

![Virtual Machine Comparison with Containers](/images/vms_vs_containers.png "Virtual Machines and Containers - Credit: www.docker.com")
 
- With Docker, you can create a single `Container`  of your application and dependencies, including the necessary parts of the filesystem. You can then drop the container or into any compatible environment (Linux, or Windows/OSX with the Docker app installed) and run it the same way without worry.
- Network settings allow you to create a docker network where you can run containers for different microservices on the same network.
- Reduced overhead for configuration has led to it becoming very popular
- Docker is fast to build. It relies on `images` either stored in the Docker Hub or built locally by you to quickly rebuild containers

### Install Docker
  - [Mac OS: Docker for Mac](https://docs.docker.com/docker-for-mac/install/)
  - [Windows: Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
  - [Linux](https://docs.docker.com/engine/installation/linux/)
  
### Basic Docker / Docker Compose commands:
  - `docker-compose build`            : Build or rebuild services
  - `docker-compose up`               : Create and start containers
  - `docker-compose down`             : Stop and remove containers, networks, images, and volumes
  - `docker-compose ps`               : List containers
  - `docker-compose exec`             : Execute a command in a running container
  - `docker-compose logs`             : View output from containers
  - `docker-compose build --pull`     : Pull service images
  - `docker-compose restart`          : Restart services
