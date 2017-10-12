# Basic Vagrant/Ansible starter script for Rails

## 1. Getting started

### Pre-setup

#### Install [Homebrew](https://brew.sh)

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

#### Install [Ansible](https://www.ansible.com)

`brew install ansible`

#### Intall [Vagrant](https://www.vagrantup.com)

`brew install vagrant`

#### Install [VirtualBox](https://www.virtualbox.org)

You'll first need to brew "tap" cask:

`brew tap caskroom/cask`

Then install VirtualBox:

`brew cask install virtualbox`

### Build your Vagrant box

#### Clone this repo

`git clone git@github.com:npearson72/vagrant_ansible_rails_box.git && cd vagrant_ansible_rails_box`

#### Copy/rename `.env_example` to `.env`

`cp .env_example .env`

Now edit the values in the `.env` file

#### Start the build

You can use a [dotenv Vagrant plugin](https://github.com/gosuri/vagrant-env)

-- or --

Just source your `.env` file with this command:

`sh -ac '. ./.env; vagrant up'`

Note: You'll want to source your `.env` file for other provisioning commands too, example:

`sh -ac '. ./.env; vagrant reload --provision'`

Once Ansible is finished you'll have a Vagrant box ready to go!

---

## 2. Getting your SSH keys to work

Before you SSH into Vagrant you'll to make sure your [SSH keys are forwarded to Vagrant](https://wildlyinaccurate.com/using-ssh-agent-forwarding-with-vagrant):

#### On your host machine (not in Vagrant), edit your `~/.ssh/config` file

```
host vagrant*
    ForwardAgent yes
```

#### On your host machine add your identities to the SSH agent

`ssh-add`

#### Test to make sure your SSH identities were added

`ssh-add -L`

You should get a print out of your host's `ssh-rsa` (or equivalent)

#### SSH into Vagrant from the same shell session used in the steps above

`vagrant ssh`

Once you SSH into Vagrant, you'll be dropped into: `/vagrant/projects`

This is where you'll want to clone your Rails (or other) projects and begin using them
as you would on your host machine.

#### Test to make sure your SSH identities are used by Vagrant

`ssh-add -L`

You should get a print out of your host's `ssh-rsa` (or equivalent)

---

## 3. Getting Rails up and running

Once you clone your repo(s) into `/vagrant/projects/` you'll want to perform
the typical Rails setup steps:

```
bundle
rake db:create db:migrate
# Etc...
```

Then start Rails:

```
rails s -b 0.0.0.0

# or use the alias added by this Ansible script

start_rails
```

Your Rails port `3000` within Vagrant will be mapped to `3001` in your host machine

So just point your browser to `http://localhost:3001`
