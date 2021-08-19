## How to delete all commit history in GitHub?

If you ever pushed your credentials into GitHub and you had many commits after that incident then it would be a big problem removing the credentials from the history. So one of the options would be to delete the whole commit history. In this tutorial we are going to see how to do that 😉

### Deleting the commit history

**Create a new branch**

```
git checkout --orphan latest_branch
```

**Add all the files**
```
git add .
```

**Commit the changes**
```
git commit -am "commit message"
```

**Delete the branch**
```
git branch -D main
```
**Rename the current branch to main**
```
git branch -m main
```
**Finally, force update your repository**
```
git push -f origin main
```

