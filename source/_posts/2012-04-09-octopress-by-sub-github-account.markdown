---
layout: post
title: "Octopress by sub GitHub account"
date: 2012-04-09 18:35
comments: true
categories:
---

This entry explain a very particular case. It's how to make [Octopress](http://octopress.org) page with clean commit log by sub GitHub account.

## SSH config

At first, if you use multiple GitHub accounts, you need to edit .ssh/config like below.

``` bash .ssh/config
Host github.com #main account
	IdentityFile	~/.ssh/user.pem

Host github-user2 #sub account
	IdentityFile	~/.ssh/user2.pem

Host github*
	User			git
	HostName		github.com
```

## Setup GitHub Pages

Next, you should create remote master branch. It's temporary.

``` bash
mkdir user2.github.com
cd user2.github.com
git init
touch README
git commit -am "First commit"
git remote add origin git@github-user2:user2/user2.github.com.git
git push -u origin master
```

## Setup Octopress

``` bash
cd ..
git clone git://github.com/imathis/octopress.git
cd octopress
git config --local --add user.name user2
git config --local --add user.email user2@example.com
bundle install --path=vendor/bundle
bundle exec rake setup_github_pages
```

This time, you should input a hostname of sub account.

```
Enter the read/write url for your repository: git@github-user2:user2/user2.github.com.git
```

## Change to your sub account

Octopress use "_deploy" dir for push to GitHub pages. But in a default, it set as main (global) user. So fix like below.

``` bash
cd _deploy/
git config --local --add user.name user2
git config --local --add user.email user2@example.com
git commit --amend --author='user2 <user2@example.com>'
```

Ready to deploy.

``` bash
cd ..
bundle exec rake gen_deploy
```

You can get clean history.
