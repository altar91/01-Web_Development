##### Run Server
```cmd 
python manage.py runserver
```

##### Create ENV
```cmd
python -m venv ./venv
```


##### Active ENV
```cmd
.\venv\Scripts\activate.bat
```


##### Collect Static
```cmd 
python manage.py collectstatic
```

##### Start Project
```cmd 
django-admin startproject project_name .
```


##### Start App
```cmd 
python manage.py startapp listings
```
     

##### Table Creation
```cmd 
python manage.py makemigrations table_name
python manage.py migrate  
```

##### Create Admin
```cmd 
python manage.py createsuperuser
```


# Django Deployment to Ubuntu 18.04
https://gist.github.com/bradtraversy/cfa565b879ff1458dba08f423cb01d71
In this guide I will go through all the steps to create a VPS, secure it and deploy a Django application. This is a summarized document from this [digital ocean doc](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04)

Any commands with "$" at the beginning run on your local machine and any "#" run when logged into the server

## Create A Digital Ocean Droplet

Use [this link](https://m.do.co/c/5424d440c63a) and get $10 free. Just select the $5 plan unless this a production app.

# Security & Access

### Creating SSH keys (Optional)

You can choose to create SSH keys to login if you want. If not, you will get the password sent to your email to login via SSH

To generate a key on your local machine

```
$ ssh-keygen
```

Hit enter all the way through and it will create a public and private key at

```
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

You want to copy the public key (.pub file)

```
$ cat ~/.ssh/id_rsa.pub
```

Copy the entire output and add as an SSH key for Digital Ocean

### Login To Your Server

If you setup SSH keys correctly the command below will let you right in. If you did not use SSH keys, it will ask for a password. This is the one that was mailed to you

```
$ ssh root@YOUR_SERVER_IP
```

### Create a new user

It will ask for a password, use something secure. You can just hit enter through all the fields. I used the user "djangoadmin" but you can use anything

```
# adduser djangoadmin
```

### Give root privileges

```
# usermod -aG sudo djangoadmin
```

### SSH keys for the new user

Now we need to setup SSH keys for the new user. You will need to get them from your local machine

### Exit the server

You need to copy the key from your local machine so either exit or open a new terminal

```
# exit
```

You can generate a different key if you want but we will use the same one so lets output it, select it and copy it

```
$ cat ~/.ssh/id_rsa.pub
```

### Log back into the server

```
eval "$(ssh-agent)"
ssh-add ./.ssh/id_rsa_do
$ ssh root@YOUR_SERVER_IP
```

### Add SSH key for new user

Navigate to the new users home folder and create a file at '.ssh/authorized_keys' and paste in the key

```
# cd /home/djangoadmin
# mkdir .ssh
# cd .ssh
# nano authorized_keys
Paste the key and hit "ctrl-x", hit "y" to save and "enter" to exit
```

### Login as new user

You should now get let in as the new user

```
$ ssh djangoadmin@YOUR_SERVER_IP
```

### Disable root login

```
# sudo nano /etc/ssh/sshd_config
```

### Change the following

```
PermitRootLogin no
PasswordAuthentication no
```

### Reload sshd service

```
# sudo systemctl reload sshd
```

# Simple Firewall Setup

See which apps are registered with the firewall

```
# sudo ufw app list
```

Allow OpenSSH

```
### sudo ufw allow OpenSSH
```

### Enable firewall

```
# sudo ufw enable
```

### To check status

```
# sudo ufw status
```





