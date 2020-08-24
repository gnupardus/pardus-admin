# Pardus & Ansible

To install Ansible (Debian, Pardus);

```
sudo apt update -y

sudo apt install ansible -y

```
Then this computer will generate ssh-key and pass that ssh-key to clients.

```
ssh-keygen -t rsa

Enter file in which to save the key (/home/pardus/.ssh/id_rsa):
/home/pardus/.ssh/id_rsa already exists.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/pardus/.ssh/id_rsa.
Your public key has been saved in /home/pardus/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:EgUoPrRq9lFTNoWfVnm9MC+gGPOQnXHhJ4voCOiEq+s pardus@ansible-server
The key’s randomart image is:
+—[RSA 2048]—-+
| …*oooo . |
| o . @ oo+ + . |
| o o + O +oo.+ .|
|. = o o.*. +. o |
|.+ o. o.S. . . |
|++ .. o. |
|+.. .. . |
|. . |
|+E |
+—-[SHA256]—–+
```

With "/home/pardus/.ssh/id_rsa", the path where the ssh-key will be generated will be choosen, we proceed without changing the standard naming "id_rsa". Let's continue the "passphrase" question without entering any password, we will not connect with the password since we will send a key.

Then we need to send this generated key to the client.

```
ssh-copy-id istemci@ip_adresi
```

via this command we send our ssh-key to our client

Note: If we have been pass our key to our client once and is required to be renewed, the -f parameter should be used.

```
ssh-copy-id -f istemci@ip_adresi
```

we run the command above for that reason 

Ansible keeps its standard files under the /etc/ansible/ directory, we will create a directory to work with;

```
cd ~

mkdir ansible && cd ansible

cp -rf /etc/ansible/* .

pardus@ansible-server:~/ansible$ ls -la
total 32
drwxr-xr-x 2 root root 4096 Aug 13 18:58 .
drwxr-xr-x 6 pardus pardus 4096 Aug 14 06:05 ..
-rw-r–r– 1 root root 20265 Aug 13 18:54 ansible.cfg
-rw-r–r– 1 root root 1022 Aug 13 18:58 hosts
```

the "inventory" value in the ansible.cfg file;

```
inventory = ./hosts
```

change as written above 

Then into the "hosts" file;

```
[parduslar]
192.168.122.86

```

We add hosts in the form. Multiple clients can be added one under the other as ip or hostname. Likewise, similar clients can be grouped. For example CentOS clients;

```
[centos]
192.168.122.42
192.168.122.14
192.168.122.57
```

can be added in the form. In this way, the task to be sent can be forwarded according to this grouping. In other words, the same task can be sent to CentOS clients, separately to Ubuntu clients, or to all clients with the option of all.

Let's send a task, for this while we are in the Ansible directory;

```
ansible -i hosts -m ping all
```

We can ping with that command. The output of this command;

```
192.168.122.86 | SUCCESS => {
“changed”: false,
“ping”: “pong”
}
```

will be written above.

```
ansible -i hosts -a “ip a” all
```

With the "-a" parameter, commands can be executed on remote clients. Instead of the "all" parameter;

```
ansible -i hosts -a “ip a” parduslar
```

this command only applies to clients in the parduslar group.

Kaynak : [pardus-ansible](https://dev.to/farukomercakmak/pardus-ansible-237g)


Translation Notes : Translation is made by Mert Gör(hwpplayer1). For misinformation or typo please create an issue on this repository
