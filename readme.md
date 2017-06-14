# Create python virtualenv on control node

    > cd ~/virtualenvs/
    > virtualenv ansible
    > source bin/activate
    > pip install --upgrade pip
    > pip install --upgrade setuptools
    > pip install ansible

# Install Dependancies on control node

    > ansible-galaxy install dev-sec.os-hardening  
    > ansible-galaxy install dev-sec.ssh-hardening

# Generate the ssh keys on control node

    > ssh-keygen -t rsa -b 4096 -C "rik" -f id_rsa_rik
    > ssh-keygen -t rsa -b 4096 -C "superman" -f id_rsa_superman

# Initial run

Assume a default ubuntu server 16.04 installed with only ssh-server and a user 'superman' and known password

    > ansible-playbook site.yml --ask-pass  --ask-sudo-pass

The initial run sets up the users, sudo (without password for superman), hardens the os and ssh setup

# Testing
    > ssh -i .ssh/id_rsa_superman_asible superman@<hostname>
    > ssh -i .ssh/id_rsa_rik rik@<hostname>

# Second run

You can now ssh to the machine with ssh keys. So use this when installing

    >  ansible-playbook site.yml --private-key .ssh/id_rsa_superman

# Production run

There are certain things (like letsencrypt certificate generation/renewal) that
can only be done in productions. For this a extra variable must be passed:

    >  ansible-playbook site.yml --private-key .ssh/id_rsa_superman --extra-vars "production=true"
