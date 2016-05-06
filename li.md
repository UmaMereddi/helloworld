#Lift-off  

When the instance first boot, we need to inject some userdata(password, sshkey) to instance, resize the root partition, configure OS (including install applications or update the OS)

We can pass the data (or bash command) to instance and cloud init in the vm will read the data (or command), configure the data.

[cloud init:](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html)

cloud init requires 2 files,**userdata** and **metadata**.
`Userdata` is user specific and `metadata` is related to the instance.

For injecting the root password (or any user data), we need to pass the user data when create vm (with out metadata service, OpenStack will put the user data and ssh-key in config drive)

In the command line, we can create a user-data file (myfile.txt):

###$ vi myfile.txt  
\#cloud-config  
password: mysecret  
chpasswd: { expire: False }  
ssh_pwauth: True  

###when spown the vm:

###$ nova boot ubuntu_darcy --flavor 2 --user-data=./myfile.txt --block-device source=image,id=b1d5f899-bb55-4136-9587-493ca863de52,dest=volume,size=20,shutdown=preserve,bootindex=0

[https://ask.openstack.org/en/question/60803/any-openstack-centos-image-with-a-set-password-i-can-use/]

If you are using novaclient api to inject the user data, we need to pass the string (contents of the myfile.txt) to "userdata" option.


