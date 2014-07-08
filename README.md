pubstack
========

Publisher's DevStack

## Installation

1. Install [Vagrant](http://www.vagrantup.com/).
1. Install [VirtualBox](https://www.virtualbox.org/).
1. Install Ansible:
    - [Latest Releases Via Homebrew (Mac OSX)](http://docs.ansible.com/intro_installation.html#latest-releases-via-homebrew-mac-osx))
    - [Latest Releases Via Apt (Ubuntu)](http://docs.ansible.com/intro_installation.html#latest-releases-via-apt-ubuntu)
    - [See all](http://docs.ansible.com/intro_installation.html#installing-the-control-machine)
1. Add project files to your machine (for example in `~Sites/www`).
1. Clone this repo:

    ```bash
    git clone git@github.com:NBCUOTS/pubstack.git
    cd pubstack
    ```

1. Create your config file from the default template and modify as needed (note each config has commented instructions):

    ```bash
    cp default.config.yml config.yml
    vim config.yml
    ```

1. Start-up your vagrant box:

    ```bash
    vagrant up
    ```

1. Modify your `hosts` file with an entry for pubstack and any additional domains defined in the sites array in your `config.yml` file:

    ```bash
    sudo echo "172.20.20.10 pubstack" >> /etc/hosts
    ```

1. Visit [http://pubstack/](http://pubstack/) in your browser.

## Authors
- breathingrock
- conortm
- ericduran
- scottrigby