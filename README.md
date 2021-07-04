# **TrustedBSD-Installing-Biba-Policy**
<br><br>


***Step 1.*** Install FreeBSD and make sure to install the UFS filesystem option


***Step 2.*** Open up the ```"/boot/loader.conf"``` file and put these in there:
```
mac_biba_load="YES"
security.mac.biba.trust_all_interfaces=1

```


***Step 3.*** Edit the ```"/etc/fstab"``` and set the root partition to ```"ro"``` and then reboot.


***Step 4.*** Run tunefs -l enable /  and reboot after


***Step 5.*** As the root user run ```mount -urw /``` and change the ```"ro"``` back to ```"rw"``` in the /etc/fstab and then reboot again.


***Step 6.*** Edit the ```"/etc/login.conf"``` towards the top for our default user and make sure it looks like this below. The line we are specifically adding to this file is this

```
:label=biba/high(high-high):\
```

The whole ```"default"``` section should look like this now:

```
default:\
	:passwd_format=sha512:\
	:copyright=/etc/COPYRIGHT:\
	:welcome=/etc/motd:\
	:setenv=MAIL=/var/mail/$,BLOCKSIZE=K:\
	:path=/sbin /bin /usr/sbin /usr/bin /usr/games /usr/local/sbin /usr/local/bin ~/bin:\
	:nologin=/var/run/nologin:\
	:cputime=unlimited:\
	:datasize=unlimited:\
	:stacksize=unlimited:\
	:memorylocked=64K:\
	:memoryuse=unlimited:\
	:filesize=unlimited:\
	:coredumpsize=unlimited:\
	:openfiles=unlimited:\
	:maxproc=unlimited:\
	:sbsize=unlimited:\
	:vmemoryuse=unlimited:\
	:swapuse=unlimited:\
	:pseudoterminals=unlimited:\
	:priority=0:\
	:ignoretime@:\
	:umask=022:
	:label=biba/high(high-high):\
	:charset=UTF-8:\
	:lang=C.UTF-8:
  
  ```
  
  
  ***Step 7.*** Continue to edit the ```"/etc/login.conf"``` file and scroll all the way to the bottom and add ```"myuser"``` like so:
  
  ```
  myuser:\
	:label=biba/low(low-high):\
	:tc=default:
  
  ```
  
  
  ***Step 8.*** Once you are done editing the ```"/etc/login.conf"``` file we will now map our users to the users in the file. First we will map our regular user named ```'john'``` to our ```'myuser'``` in the file like so:
  
  
```
pw usermod john -L myuser

```

By doing this our user John whenever he logs into the system will have a clearance of ```"biba/low(low-high)"```


***Step 9.*** Now we will map our root user to the default user in ```"/etc/login.conf"``` like so:

```
pw usermod root -L default
```


By doing this our root user whenever he logs into the system will have a clearance of ```"biba/high(high-high)"```
