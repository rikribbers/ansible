# Install Dependancies

    > ansible-galaxy install dev-sec.os-hardening  
    > ansible-galaxy install dev-sec.ssh-hardening 
    > ansible-galaxy install jdauphant.NGINX 

# Initial run

Assume a default ubuntu server 16.04 installed with only ssh-server and a user 'superman' and known password

    > ansible-playbook site.yml --ask-pass  --ask-sudo-pass 

The initial run sets up the users, sudo (without password for superman), hardens the os and ssh setup

# Testing 
    > ssh -i .ssh/id_rsa_superman_asible superman@<hostname>
    > ssh -i .ssh/id_rsa_rik rik@<hostname>


# Second run

You can now ssh to the machine with ssh keys.

    >  ansible-playbook site.yml --private-key .ssh/id_rsa_superman_ansible

