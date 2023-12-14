# Setting up an initial repo

## Create repo in Github
From the UI in Github, Go to Repositories and click on the New button 
after entering your name in the text field.

Be sure to create an empty repo with no REPO for these instructions.

##
git clone using the ssh option for the repository url because
we set up the ssh key in /instructions/getting-started-ssh
```
git clone git@github.com:rejoicealways/nameofrepo.git

```

If you haven't already, you need to set your name and email using:
```
git config --global --edit
```
Change the name to be your username in github
Change your email to the one associated with your account

Next, I wanted to change the name of the default branch I was working with
to master because that is what I was used to.

```
echo "# sample text" >> README.md
git add README.md
git commit -m "setting up repo"
git branch -M master
git push -u origin master
```

You will be prompted for any passphrase you have set up for the SSH Key if 
you did that on ssh key creation.


