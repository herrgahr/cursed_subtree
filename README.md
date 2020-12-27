Cursed example of using git-config's include feature to:

1. track remotes in the repository itself
2. abuse the remote's name to encode/a/subtrees/prefix
3. use aliases to add / pull those subtrees

To kick things off, you need to tell git-config to include .git-config-include from the workdir:

```
git config include.path ../.git-config-include
```

`git remote -v` should now list the remotes configured in .git-config-include


Additionally, each remote includes the bogus remote.foo.subtree value which can be used to filter out those magic remotes from normal ones:

```
$ git config --get-regexp 'remote\..+\.subtree'|cut -d. -f2

fmt
luajit
sub/luajit
fmt/luajit
```

...of course, there's an alias (stl) for this in .git-config-inlcude ;)

...which can be used for bulk pulls:

```
# list all subtrees and perform `git stu` for each
git stl | xargs -L1 git stu
```


