## Set up 3 VMs 

- Set up 3 VMs with the following config 

```
 -*- mode: ruby -*-
 # vi: set ft=ruby :
 
 # All Vagrant configuration is done below. The "2" in Vagrant.configure
 # configures the configuration version (we support older styles for
 # backwards compatibility). Please don't change it unless you know what
 
 # MULTI SERVER/VMs environment 
 #
 Vagrant.configure("2") do |config|
    # creating are Ansible controller
      config.vm.define "controller" do |controller|
        
       controller.vm.box = "bento/ubuntu-18.04"
       
       controller.vm.hostname = 'controller'
       
       controller.vm.network :private_network, ip: "192.168.56.11"
       
       # config.hostsupdater.aliases = ["development.controller"] 
       
      end 
    # creating first VM called web  
      config.vm.define "web" do |web|
        
        web.vm.box = "bento/ubuntu-18.04"
       # downloading ubuntu 18.04 image
    
        web.vm.hostname = 'web'
        # assigning host name to the VM
        
        web.vm.network :private_network, ip: "192.168.56.12"
        #   assigning private IP
        
        #config.hostsupdater.aliases = ["development.web"]
        # creating a link called development.web so we can access web page with this link instread of an IP   
            
      end
      
    # creating second VM called db
      config.vm.define "db" do |db|
        
        db.vm.box = "bento/ubuntu-18.04"
        
        db.vm.hostname = 'db'
        
        db.vm.network :private_network, ip: "192.168.56.13"
        
        #config.hostsupdater.aliases = ["development.db"]     
      end
    
    
    end
```

- Once all 3 are running open 3 terminals SSH into each VM and run `sudo apt-get update -y && sudo apt-get upgrade -y`

---

- Once completed follow the following steps:
  
1. SSH into Controller VM
2. Run Update and Upgrade
3. Run `sudo apt-get install software-properties-common`
4. Run `sudo apt-add-repository ppa:ansible/ansible`
5. Run `Sudo apt-get update`
6. Run `sudo apt-get install ansible -y`
7. Check `sudo apt-get install tree `
8. cd into `cd /etc`
9.  cd into `cd ansible/`
10. pwd = /etc/ansible
11. Now we want to ssh into web vm from insode the controller vm
12. Enter `sudo ssh vagrant@192.168.56.12` enter then password `vagrant`
13. you should now be inside the web vm 
14. To return back to controller enter `exit`
15. Now we want to ssh into web db from insode the controller vm
16. Enter `sudo ssh vagrant@192.168.56.12` enter then password `vagrant`
17. **From the controller vm we would like to check if we can speak to the web and db vms**
18. To do so follow the following steps 
19. Run `sudo nano hosts`
    
20. Inside the hosts folder add the following 
     ``` 
    [web]
    192.168.56.12 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
    [db]
    192.168.56.13 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
    ```
    - add screenshot
21. Test by running `sudo ansible ping -m all ` or `sudo ansible -m ping web` or `sudo ansible -m ping db`
22. `sudo ansible all -a "sudo apt update"`
    