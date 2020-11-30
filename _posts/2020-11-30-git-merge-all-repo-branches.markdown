---
layout: post
title:  "Merge All Git Repo. Branches Locally"
date:   2020-11-30 05:39:00 -0500
categories: iac 
tags: git
---
Currently the team I work with have a uniform repository branch strategy that is a two tiered *master* and *development* branch to merge in code changes to our production environment. Features/fixes also have their own branches that are short lived and related to the JIRA ticket being worked on (e.g. *NEB-99999*). Once the changes are merged into *development* and then *master* the branch is closed out. 

A typical workflow for me then, is that any changes I make are generally on a branch that is mine, until I push those changes into *development*. However, it can be days/weeks before I come back to a particular repository and others could have made many changes since then. Below is a simple script that I keep in my *{local_repo_copy}/test/* directory to quickly update the *master* and *development* branch each time I come back to work on a repository. This way I quickly get my local repository updated with any changes that have occurred and I can ensure I am working against the current code base. 

{% highlight bash %}
#!/bin/bash 

BRANCHES="master development"

cd ..
for BRANCH in $BRANCHES; do 
  printf '\n'
  echo "Updating $BRANCH for repo $(pwd)"
  git checkout $BRANCH && git pull origin $BRANCH --ff-only
done
{% endhighlight %}
