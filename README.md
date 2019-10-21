# git-aliases
Usefull git aliases

#### Create local credential storage
```bash 
git config credential.helper store
```

#### Get first remote name (via git -remote)
```bash 
git config --global --replace-all alias.getfirstparam "!f() { echo $1;}; f"
git config --global --replace-all alias.firstremote "!f() { export remotes=`git remote -v`; export remote_name=`git getfirstparam $remotes`; echo $remote_name;}; f"
```

Example:
```bash 
git firstremote
```

#### Create new branch from current, and push to remote repo
```bash 
git config --global --replace-all alias.newbranch "!f() { export remote_name=`git firstremote`; git checkout -b $1; git push $remote_name $1; }; f"
```

Example:
```bash 
git newbranch my_awesome_branch/2-4
```

#### Checkout branch + pull
```bash 
git config --global --replace-all alias.goto "!f() { git checkout $1 -f; git qpull; }; f"
```

Example:
```bash 
git goto master
```

#### Quick (lazy) push
```bash 
git config --global --replace-all alias.qpush "!f() { export remote_name=`git firstremote`; export branch_name=`git rev-parse --abbrev-ref HEAD`; git push $remote_name $branch_name; }; f"
```

Example:
```bash 
git qpush
```

#### Quick (lazy) pull
```bash 
git config --global --replace-all alias.qpull "!f() { export remote_name=`git firstremote`; export branch_name=`git rev-parse --abbrev-ref HEAD`; git pull $remote_name $branch_name; }; f"
```

Example:
```bash 
git qpull
```

#### Quick (lazy) fetch whith -p parameter
```bash 
git config --global --replace-all alias.qfetch "!f() { export remote_name=`git firstremote`; git fetch $remote_name -p; }; f"
```

Example:
```bash 
git qfetch
```

#### Quick commit with comment and push
```bash 
git config --global --replace-all alias.qcom "!f() { export remote_name=`git firstremote`; export branch_name=`git rev-parse --abbrev-ref HEAD`; git commit -m \"$\"; git push $remote_name $branch_name; }; f"
```

Example:
```bash 
git qcomm "init commit"
```

#### Quick fixmerge: commit with comment "fix merge" and push
```bash 
git config --global --replace-all alias.fixmerge "!f() { export remote_name=`git firstremote`; export branch_name=`git rev-parse --abbrev-ref HEAD`; git commit -m \"fix merge\"; git push $remote_name $branch_name; }; f"
```

Example:
```bash 
git fixmerge
```

#### Quick create tag for release/hotfix (pull in current branch, create tag with custom name and push to remote repo
```bash 
git config --global --replace-all alias.at "!f() { export remote_name=`git firstremote`; export branch_name=`git rev-parse --abbrev-ref HEAD`; git pull $remote_name $branch_name; git tag -a $1 -m \"$1\"; git push $remote_name $1; }; f"
```

Example:
```bash 
git at hotfix20012011
git at release20012011
```

#### Drop dev branch, recreate from master branch
```bash 
git config --global --replace-all alias.redev "!f() { export remote_name=`git firstremote`; export tmp_branch=`git rev-parse --abbrev-ref HEAD` && git checkout -f master;git pull $remote_name master;git push --delete $remote_name $1;git branch -D $1;git newbranch $1; git commit --allow-empty -m \"recreate branch $1 from master\"; git push $remote_name $1; }; f"
```

#### И, немножко упорки:
Вмержить все изменения из текущей ветки в заданную ветку( например влить свои правки из своей ветки в ветку dev), в конечном итоге остаемся в своей же ветке:
```bash 
git config --global --replace-all alias.mergeto "!f() { export remote_name=`git firstremote`; export tmp_branch=`git rev-parse --abbrev-ref HEAD` && git checkout -f $1;git pull $remote_name $1; git merge $tmp_branch; git push $remote_name $1;git checkout $tmp_branch; }; f"
```

Example:
```bash 
git mergeto <ветка>
git mergeto dev
```
Внимание! При возникновении мерж конфликтов при вливе в результирующую ветку срипт стопнется, придется руками исправлять мерж-конфиликты (окошко с мерж-конфликтами не будет) и пушить в результирующую ветку
