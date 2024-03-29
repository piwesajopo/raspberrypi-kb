# How to enable SSH on Raspbian

### Run Raspberry Pi Configuration Tool and enable SSH
```
# After executing the following command, go to Interface Options -> SSH 
sudo raspi-config
```

### Create a private and public key files for SSH login on other systems
```
# Generate key files. The .ssh folder will be created
ssh-keygen -t rsa -b 4096 -C "moisoto.jr@gmail.com"

# Make sure .ssh has the right permissions
chmod 700 .ssh
```

### Connecting from other computers

Create authorized_keys file then add the public keys of client machines that you want to be able to connect to your Raspberry Pi:

```
touch ~/.ssh/authorized_keys

# It should be created with 644 permissions, but just to make sure
chmod 644 ~/.ssh/authorized_keys

# Use an editor to put public keys into the file, or just add them if you downloaded them from an email or other source:
cat some_public_key.pub >> ~/.ssh/authorized_keys
```

### Disabling Password Authentication (only allow connections using certificate)

Edit sshd_config file 
```
sudo vi /etc/ssh/sshd_config
```

Look for this section and change the ***PasswordAuthentication*** entry to ***no***
```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
#PasswordAuthentication yes
#PermitEmptyPasswords no
```

Now restart the *sshd* service
```
sudo systemctl restart sshd
```
