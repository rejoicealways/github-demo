#Absolute beginner Git Instructions

## Set up Git authentication

Create an account on GitHub using your email and unique user name [Git Hub](https://github.com)

Turn on two-factor authentication for security before setting up your repositories or any repositories
shared with you.

Go to your `Profile Image -> Settings -> Passwords and Authentication`
Enable two-factor.  I would recommend using an authenticator app.


Generate a ssh key using `ssh-keygen` on a Mac or Linux.  For Windows, you need to install a Linux shell emulator like Cmder and it will have this option
```
ssh-keygen -f ~/<path_to_new_file_key> -t rsa
or
ssh-keygen -f ~/<path_to_new_file_key> -t ed25519
```
Note:if you plan to use this ssh key for authentication mysql workbench, I would recommend
`rsa` because I at the time of this writing, that is what mySQL workbench supported.



This will generate by default in your `.ssh` directory in your ~ directory on your system on Mac and Linux. 
On Windows, it will be in your `c:\Users\username\.ssh`

Copy the PUBLIC key to git so that it can recognize.  The public key ends with `.pub` 
Copy it to `Profile Image->Settings ->SSH keys`

If you have several connections you are going to make with ssh with several ssh keys, you will need to edit the `~/.ssh/config` file as follows:

```
Host github.com
HostName github.com
User your-github-username
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_ed25519_myaccount_github
```
To test your ssh connection and configuration
use
`ssh -T git@github.com`

Reference: https://gist.github.com/alejandro-martin/aabe88cf15871121e076f66b65306610

