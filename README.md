django-lfs-on-openshift
=======================

1.Create an account at http://openshift.redhat.com/ , and install the client tools.

2.Create a diy-0.1 application :

    rhc app create -a lfs -t diy-0.1 -l email

3.Add MySQL support to your application

    rhc app cartridge add -a lfs -c mysql-5.1 -l email

4. SSH into the server:
    ssh $(git config --get remote.origin.url | cut -d/ -f3)
    cd $OPENSHIFT_RUNTIME_DIR
    wget http://dl.dropbox.com/u/28357022/openshift/lfs-app.tar.gz
    tar xf lfs-app.tar.gz
    cd lfs-app
    ./bin/django syncdb
    ./bin/django lfs_init
    exit

5.Add this upstream django-lfs-on-openshift repo

    cd django-lfs-on-openshift
    git remote add upstream -m master git://github.com/bjjay/django-lfs-on-openshift.git
    git pull -s recursive -X theirs upstream master
    
6.Then push the repo upstream

    git push

7.That's it, you can now checkout your application at:

    http://lfs-$yournamespace.rhcloud.com


NOTES:
=====

Generate ssh public key for existed openshift account:

        #create keys
        $ ssh-keygen -t rsa -f ~/.ssh/libra_id_rsa -C email  
        #add public key
        $ rhc sshkey add -i public_key_name -k ~/.ssh/libra_id_rsa.pub  -l email
        #edit your ssh config   
        $ cat ~/.ssh/config
        Host *.rhcloud.com
          IdentityFile ~/.ssh/libra_id_rsa
          VerifyHostKeyDNS yes
          StrictHostKeyChecking no
          UserKnownHostsFile ~/.ssh/libra_known_hosts
