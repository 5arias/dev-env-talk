# Development Environments
* Why use dev environments other than your machine's OS?
    - Your application (probably) won't be deployed on Windows/OSX.
    - Your hardware does not match the server's.
    - To be certain of performance and **configuration** we develop on environments
    as close to the deployed environment as possible.
* Tools your app probably is deployed with (not mutually exclusive):
    - Amazon Web Services (AWS)
    - Docker / Docker Compose
    - Vagrant/Packer (configured system images)
* Tools for configuration
    - To make sure all envs are configured the same way, you might be using some of these tools:
        - Chef
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
  - Vagrantfile is written in Ruby.
* customize your configuration in vagrant.yml (your team might not use this)
  - Externalizing custom configuration from the Vagrantfile allows for continuity when your team makes changes to Vagrant configuration.
  - `synced_folder` needs correct `localpath` and `guestpath` to use our Ruby config in the Vagrantfile.
  - proxies may be used if you are working with a VPN
  - you may beef up your VM by increasing the CPUs and memory if you need.
    - Vagrant / VirtualBox reserve can reserve resources on your machine. Disk space is used as needed unless an upper-bound is provided.
    - Resources should be tuned to reflect the production environment for your application, or can be used to reduce the load on your local system if you are
    running on less performant hardware like a Macbook Air or similar machine.
* `$ vagrant up` and wait... it is probably downloading Ubuntu...
* `$ vagrant ssh` and you're in.
* `$ cd /vagrant_data/` to see your shared folders.

### Workflow
  * After starting and connecting to your machine, navigate to the working directory (/vagrant_data)
  * start your application in development mode, or whatever it is you do.
    - You may need to install any dependencies (`bundle install`, `npm install`, etc) first.
    - Your team will probably have truly external dependencies (correct versions of languages, package managers, system software, etc.) managed in by a tool like Chef, Puppet, or build scripts that are invoked by the Vagrantfile (same in Dockerfile)
  * On your local machine, open the same directory with your IDE or favorite text editor.
    - Many teams have preferred IDEs or text editors for their projects and may have customized configurations you can use :)
  * You will be working directly on your local machine. Changes will be immediately picked up by the VM because folders are shared.
  * Any tests, compilers, or dev servers should be run on your VM.
  * Your local machine should only be used to run your IDE/editor and manage the git repo.
    - If you set up your VM with your GitHub credentials, you can manage git from either machine.

### Basic Vagrant commands:
  * Vagrant documentation: https://www.vagrantup.com/docs/
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
