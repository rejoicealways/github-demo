#Getting started with Development as a Collaborator to a GitHub Project

```
git clone <ssh path to repository
```

Create a branch off of master in your local repository.  Be sure to name the branch something that would make
sense to other developers because when you push to the remote repository, the name will show up.  
For example. hotfix/<TaskID>Desc
Note: the / in the name of the repository does NOT create a folder or child repository, it is simply a naming
convention.

```
git branch <new branch name>
```

IMPORTANT checkout the branch you created.  If you don't, you will be making changes on master branch
```
git checkout <new branch name>
```

Use git to add new files created and modified files that are to be a part of the commit.
```
git add <untracked file>
```


```
git commit -m "Message about the commit"
```

```
git push
```
If the branch has not been created remotely yet, this will output that the following command needs to be done before
pushes can be done.

```
git push --set-upstream origin testbranch
```

