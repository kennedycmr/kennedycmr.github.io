---
layout: post
title:  "Push Code Updates to GitHub Using SSH Keys Only"
date:   2020-11-29 17:17:00 -0500
categories: jekyll update
---
When working with Git repositories that are hosted on github.com, once you start making code changes and wanting to write those back, having to constantly type your username + password gets old. This is how to setup your workstation that us running Linux, to use SSH keys (without a passphrase). 

# Create Your SSH Keys 
- On a terminal running on your workstation, running under your user ID, run the following commands: 
{% highlight bash %}
ssh-keygen -t rsa 
{% endhighlight %}

- Follow the prompts to specify the filename and location to store your keys. 
- When prompted for a passphrase, just press enter (twice) if you don't wish to secure your keys with a passphrase (not recommended)
- You should now have 2 files created as follows:  
  - ~/.ssh/filename.id_dsa    # this is your private key. It is important you keep this secure and not share. 
  - ~/.ssh/filename.id_dsa.pub   # this is your public key that goes with your private key. This key you share with vendors. 

# Update Github Repository Settings with your Public Key
- Login to Github.com
- Navigate to your profile settings -> SSH and GPG Keys
- Add a new SSH key as follows: 
  - Provide a title for your key of your choosing.
  - Paste in the contents of your "public" key created above. The line should start with *ssh-rsa*
- Click *Add SSH Key* button. 

# Update SSH Config on your Workstation
- Edit your ssh config file. (default:  ~/.ssh/config)
- Create a block as follows, ensuring you substitute in the path to your private key and the repository you are working with. 
{% highlight bash %}
host github.com-myrepo
  HostName github.com
  User git
  IdentityFile ~/.ssh/git-myprivatekey.id_rsa
{% endhighlight %}

# Update your Local Repo Clone Configuration
- Assuming you have cloned your repository to ~/repo, open the file ~/repo/.git/config
- Update the git config file, by changing the *url* line as follows (note: below is the commented original followed by the updated *url* line): 
{% highlight bash %}
host github.com-myrepo
[remote "origin"]
  #url = https://github.com/somename/repo.github.io.git
  url = git@github.com-myrepo:somename/repo.github.io.git
  fetch = +refs/heads/*:refs/remotes/origin/*
{% endhighlight %}
- Notice in the above, the URL line now contains the same name given to the *host* line in the ssh config earlier "github.com-myrepo". By using this method, we can have multiple git config blocks that all point to github.com, yet, refer to different repositories and be able to have different ssh keys. 

# Test
- Now when you perform a *git push* command, you should not be prompted for any username or password. 