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
        - Chef
        - Puppet
        - Docker
        - custom scripting!

## Vagrant with VirtualBox
Vagrant uses other virtual machine providers (like VirtualBox, Parallels, or AWS) to create customized virtual machines. 
Can configure OS, # CPUs, Memory, Disk, shared directories (with your local machine), open/closed ports, custom IPs, etc.
* Install VirtualBox
* Install Vagrant
* Install Chef DK
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
* What is Docker?
    - Docker is like Linux cgroups. ...
        - Docker is NOT a VM, it is a collection of filesystem images layerd on top of eachother and sharing resources.
        The base Docker image is a barebones Linux kernel. Everything else is added as needed.
    - It is a tool for layering application resources in a modular fashion to minimize disk space
    and improve your app's portability.
    - The dream of Docker is to create a single `Container` or `Image` of your application and ALL of it's
    dependencies, including the neccesary parts of the filesystem. You can then drop this Container into any
    compatible environment (Linux, or Windows/OSX with the Docker app installed) and run it the same way without worry.
    - There are downsides to using Docker, but the reduced overhead for configuration has led to it becoming very popular.

