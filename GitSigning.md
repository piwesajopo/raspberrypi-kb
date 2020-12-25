# How to configure git account for commit signing

*Taken From:* [Github About Commit Signature Verification](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/about-commit-signature-verification)

### Creating a GPG Key and loading it into Github

When generating a key keep in mind:
- Key size must be 4096 bits
- I preffer a non-expiring key
- Use your GitHub address, if you are keeping your mail private use your no-reply address supplied by Github.
- Load your key (shown by the last command in the following code) into your github account as detailed [here](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)

```
# List Secret Keys
gpg --list-secret-keys --keyid-format LONG

# Generate a new Key if none present
gpg --full-generate-key

# List Secret Keys
gpg --list-secret-keys --keyid-format LONG

# Prints the GPG key ID, in ASCII armor format
# Here 3AA5C34371567BD2 is used because the previous command showed: 
#     sec   rsa4096/3AA5C34371567BD2
gpg --armor --export 3AA5C34371567BD2
```

# Configuring Git for Globally using the key to commit

```
# List Secret Keys
gpg --list-secret-keys --keyid-format LONG

# Tell git to use the key to sign commits
git config --global user.signingkey 3AA5C34371567BD2

# Tell git your mail address and your name
git config --global user.email "76591915+piwesajopo@users.noreply.github.com"
git config --global user.name "Moises Soto Jr."

# Tell git to sign your commits automatically
git config --global commit.gpgsign true
```
