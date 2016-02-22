# eisen and vagrant setup(Windows)

* Install Oracle VM Virtualbox  
* Install Vagrant

for both eisen front and eisen agent:
```
   vagrant box add precise64 http://files.vagrantup.com/precise64.box
   vagrant up
```

## machine information
The vagrant machine are already setted as follow 
* eisen_front  
SSH address: 127.0.0.1:2222 (the port can be different depend on vagrant)  
SSH username: vagrant  
SSH password: vagrant  
SSH auth method: private key  

* eisen_engine  
SSH address: 127.0.0.1:2200  (the port can be different depend on vagrant)  
SSH username: vagrant  
SSH password: vagrant  
SSH auth method: private key  

## eisen start up settings
http://localhost:8080/init-setup1.php from this link you can initialize the settings of eisen.
* connection setting  
database settings  
IP address： localhost  
username：root  
password：password  
database：eisen  
・user setting  
you can use your own username email and password.

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
