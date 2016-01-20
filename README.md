# My own version of capi5k-openstack

This repo is a clone of this one: https://github.com/capi5k/capi5k-openstack. The main difference is that it contains the *xpm_modules* folders. This one is tricky to get with `xpm install` on [Grid5000](http://www.grid5000.fr). 

## What it does

It deploys OpenStack on Grid5000 with 5 nodes for a duration of 7 hours on the Toulouse's site. 

In the future, it will automatically deploy the FRIEDA framework to manage data-intensive scientific applications and it will also install my own Python application which will handle the deployement of Virtual Machines (VM) on Grid5000 depending on the user's needs. 

**Update:**

 - Automation of FRIEDA deployment: Nope
 - Automation of Greenerbar deployment: Done!

## Installation

You need to be connected on Toulouse Grid5000 frontend first. 

### Installation of Bundler

**Skip this if** you already have access to the `bundle` command. 

    gem install bundler --user

And now add this line to your .bash_rc: 

    export PATH=$PATH:$HOME/.gem/ruby/1.9.1/bin

Don't forget the `source ~/.bash_profile` command if you don't want to logout/login and check the installation with `bundle --version`. 

### Git cloning of the project
    
    # Clone the project
    git clone https://github.com/dgetux/capi5k-openstack.git
    # Install the dependencies
    bundle install --path ~/.gem

### xpm and restfully configurations and id_rsa.pub

To be able to do your first `cap -T` you will need to add the xpm configuration. To do so, move the folder `.xpm` to your home:

    mv .xpm ~/

It contains a _connection.rb_ file. Don't forget to **edit this file** because you won't be able to use my account! ;)

Then you will need to create a file for the restfully configuration:

    mkdir ~/.restfully
    echo "base_uri: https://api.grid5000.fr/3.0/" > ~/.restfully/api.grid5000.fr.yml

You will also need to specify your _id_rsa.pub_. Check if this file is present in ~/.ssh/. 

Now you should be able to execute the following command which list the available commands: 

    cap -T

## Deployement

Commands to execute the deployement: 

    cap automatic puppetcluster; cap puppetcluster
    cap openstack; cap openstack:bootstrap
    cap prepare # See https://bitbucket.org/davidguyon/greenerbar/

When connected to the controller node:

    cd greenerbar
    source testrc
    source env/bin/activate
    ipython main.py

To SSH connect on a running instance:

    ssh -i ~/.ssh/test_key.pem 10.42.42.42
