---
layout: post
title: codecommit multiple accounts
---

Let's say you have multiple AWS accounts to handle different repositories in CodeCommit. This is pretty common with roles in DevOps/DevSecOps/Platform.

```sh
    cat ~/.ssh/config
```

```conf
...
# Multiple AWS account accessing code commit
# clone repositories using:
# 	git clone ssh://HOST_FROM_AWS_CONFIG/v1/...
# https://gist.github.com/justinpawela/3a7056cd592d688425e59de2ef6f1da0

# ssh account X
Host platform
    User XXXXXXXXXXXXXXXXXXX
    Hostname git-codecommit.us-east-1.amazonaws.com
    IdentityFile ~/.ssh/platform_rsa

# ssh account y
Host devops
    User YYYYYYYYYYYYYYYYYYY
    Hostname git-codecommit.us-east-2.amazonaws.com
    IdentityFile ~/.ssh/devops_rsa
```

Then you can clone repositories from `platform`:

```sh
    git clone ssh://platform/v1/...
```

and from `devops` with:

```sh
    git clone ssh://devops/v1/...
```

The downside is we have to modify the cloning url every time we need a new repository, but, that's worth the time since after that, any `git` command will know which credentials and User will be needed.