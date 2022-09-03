# Lesson 6 - SSH


### via ssh agent ###

Check actual state of sshd:
```
sudo systemctl status sshd
```

Generate ssh keys
```
ssh-keygen
```


Send public key to the machine we want to access:
```
ssh-copy-id -i <path> <user>@<ip>
```



![title](images2/Capture1.PNG)



1. Enter with password:
```
ssh <user>@<ip>
```

![title](images2/Capture2.PNG)


2. Enter with key:
```
ssh -i <path> <user>@<ip>
```

![title](images2/Capture3.PNG)



3. Enter using agent: (configuration is stored in RAM, same terminal - not persistent)

Run agent:
```
eval `ssh-agent`
```

Add user identity:
```
ssh-add <path>
```

Access the machine:
```
ssh <ip>
```


![title](images2/Capture4.PNG)

Show ssh agent keys:
```
ssh-add -l
```

![title](images2/Capture5.PNG)




### via alias from config file ###

Add entry to config file:
```
sudo vi ~/.ssh/config
```
as following:
Host <alias>
    HostName <ip of the machine we are going to access>
    User <user>
    IdentityFile <path>

Save.

![title](images2/Capture6.PNG)
Check you can access the VM



```
ssh <alias>
```

![title](images2/Capture6.PNG)


Also, we can deny the password authentication to the target machine by editing the 
*/etc/ssh/sshd_config*


and set the: 

```PasswordAuthentication no```

``` PublickeyAuthentication yes ```  (this looks like in any case is enabled by default)
