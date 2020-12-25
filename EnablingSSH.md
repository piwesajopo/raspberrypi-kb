# How to enable SSH on Raspbian

### Run Raspberry Pi Configuration Tool
```
# Go to Interface Options -> SSH 
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
chmod 644 authorized_keys

# Use an editor to put public keys into the file, or just add them if you downloaded them from an email or other source:
cat some_public_key.pub >> ~/.ssh/authorized_keys
```

Edit sshd_config file 

```
cd /etc/ssh
sudo vi sshd_config
```

Look for this section and change the **PasswordAuthentication** entry to **no**
```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
#PasswordAuthentication yes
#PermitEmptyPasswords no
```
