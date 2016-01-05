


we suggest to use ssh-copy-id to the target host or removing StrictHostKeyChecking. 

```
Host *
    StrictHostKeyChecking no
```

Is now possible to start the Eisen Agent using Vagrant

in the Eisen Agent folder:
```
vagrant up
ssh vagrant@127.0.0.1 -p 2222 /usr/local/bin/nosetests -s /vagrant/tests/api_test.py
```