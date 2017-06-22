# Webserver as a service

This project is used to maintain my [personal webserver](https://crescit.nl)

It is fully configured from scratch using a default ubuntu server installation
with ssh-server. It is assumed there is a user 'superman' that can use sudo
without a password.

# Setup instructions

## Create python virtualenv on control node

    > cd ~/virtualenvs/
    > virtualenv ansible
    > source bin/activate
    > pip install --upgrade pip
    > pip install --upgrade setuptools
    > pip install ansible

## Install Dependancies on control node

    > ansible-galaxy install dev-sec.os-hardening  
    > ansible-galaxy install dev-sec.ssh-hardening
    > ansible-galaxy install angstwad.docker_ubuntu

## Generate the ssh keys on control node

    > ssh-keygen -t rsa -b 4096 -C "rik" -f id_rsa_rik
    > ssh-keygen -t rsa -b 4096 -C "superman" -f id_rsa_superman

## Initial run

Assume a default ubuntu server 16.04 installed with only ssh-server and a user
'superman' that can sudo and has a known password.

    > ansible-playbook site.yml --ask-pass  --ask-sudo-pass

The initial run sets up the users, sudo (without password), hardens the os and
ssh setup

## Connection to the server
    > ssh -i .ssh/id_rsa_superman_asible superman@<hostname>
    > ssh -i .ssh/id_rsa_rik rik@<hostname>

## Second run

You can now ssh to the machine with ssh keys. So use this when installing

    >  ansible-playbook site.yml --private-key .ssh/id_rsa_superman

# Production run

There are certain things (like letsencrypt certificate generation/renewal) that
can only be done in production. For this a extra variable must be passed:

    >  ansible-playbook site.yml --private-key .ssh/id_rsa_superman  \
    >  --extra-vars "production=true"
