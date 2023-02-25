---
layout: post
title: ssh work & personal
---

> Disclaimer: This may be highly opinionated.

With personal I meant side projects and experiments of course :wink:

The scenario is simple, we have ssh key or keys for work and for personal projects.

```sh
    ls ~/.ssh/
```

```txt
id_rsa
id_rsa.pub
personal_rsa
personal_rsa.pub
work_rsa
work_rsa.pub
```

our `~/.ssh/config` file may looks like:

```config
# https://docs.gitlab.com/ee/ssh/#per-repository-ssh-keys

Host *
AddKeysToAgent yes
UseKeychain yes

# Work
Host github.com
	Preferredauthentications publickey
	ForwardAgent yes
	IdentityFile ~/.ssh/work_rsa

Host jumpbox
	HostName SOME_IP_OR_HOST_HERE
	Preferredauthentications publickey
	ForwardAgent yes
	IdentityFile ~/.ssh/work_rsa

# Personal
Host personal
	HostName SOME_IP_OR_HOST_HERE
	Preferredauthentications publickey
	ForwardAgent yes
	IdentityFile ~/.ssh/personal_rsa

Host hostx
	User ubuntu
	HostName SOME_IP_OR_HOST_HERE
	ForwardAgent yes
	IdentityFile ~/.ssh/personal_rsa
```

Then we can login ssh:

```sh
    ssh jumpbox
```

```sh
    ssh hostx
```

We have different ssh keys and aliases to avoid remembering hosts or IPs