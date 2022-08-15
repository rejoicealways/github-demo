Show all the local and remote branches
```
git branch -a
```

Lists the files that are commited, modified, and untracked.
```
git status
```

Stashes current repository changes.  Important to note, before you can checkout another branch, you must have
a clean working directory with no modifications to be made.  This is where git stash comes in handy because you can
stash your changes and checkout a different branch for work.
```
git stash push -m "stash_name"
```

If you want to work on your changes again for a certain branch, pop the changes off of the stash stack
```
git pop <stashname>
```