---
layout: post
title: git work & personal
---

For this we will use `gitconfig` file.

```sh
    cat ~/.gitconfig
```

```conf
# This is Git's per-user configuration file.
[user]
	name = lester
	email = PERSONAL_EMAIL_HERE

[core]
	editor = vim
	attributesfile = ~/.gitattributes

[includeIf "gitdir:~/develop/"]
	path = ~/.developconfig

[includeIf "gitdir:~/sideprojects/"]
	path = ~/.sideprojectsconfig

[push]
	default = simple

[init]
	defaultBranch = main

```

If we are inside `develop` folder, then we are going to use _work_ config

```sh
    cat ~/.developconfig
```

```conf
[user]
	name = Lester
	email = COMPANY_EMAIL_HERE

```

If we are inside our side projects folder, we are going to use _personal_ config

```sh
    cat ~/.sideprojectsconfig
```

```conf
[user]
	name = lesterzone
	email = PERSONAL_EMAIL_HERE
```