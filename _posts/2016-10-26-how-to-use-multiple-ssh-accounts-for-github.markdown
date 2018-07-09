---
layout: default
title:  "How to use multiple ssh accounts for Github?"
date:   2016-10-26
categories: ssh github
---
Sometimes, you want to use multiple ssh accounts - say, one for [Github](http://github.com) and others for some other websites.

By following [generate a new ssh key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), create two different ssh keys. While generating the keys, make sure to use different file names to save the keys.
For e.g.

{% highlight bash %}
ssh-keygen -t rsa -b 4096 -C "bob@gmail.com"
# Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]

ssh-keygen -t rsa -b 4096 -C "john@gmail.com"
# Enter a file in which to save the key (/Users/you/.ssh/id_rsa): /Users/john/.ssh/id_rsa_for_github
{% endhighlight %}

Now modify the ssh `config` file
{% highlight bash %}
# when the host name is github.com-bob, use the following configuration
#- use the ssh key at ~/.ssh/id_rsa and overwrite the hostname as github.com
Host github.com-bob
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa

# when the host name is github.com-john, use the following configuration
#- use the ssh key at ~/.ssh/id_rsa_for_github and overwrite the hostname as github.com
Host github.com-john
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_for_github
{% endhighlight %}

Go back to your git local repository. If you prefer to use different user name and email address for this repository, do configure them at the repository level.
{% highlight bash %}
$ git config user.name "John"
$ git config user.email "john@gmail.com"
{% endhighlight %}

Add git remote pointing to your Github repository. Modify the hostname to the one that you used in `~/.ssh/config`. If you want to use the ssh key `id_rsa`, use `github.com-bob` as the hostname. If you want to use the ssh key `id_rsa_for_github` use `github.com-john` as hostname. In the following example, we will use `github.com-john` as hostname so that we use the ssh key `id_rsa_for_github`.

{% highlight bash %}
git remote add origin git@github.com-john:john/SampleRepo.git
{% endhighlight %}

Before you attempt to push the code, make sure to register the newly created ssh key in Github.

Now when you push the code from this repository it will try to push to `github.com-john` which will be intercepted by the ssh agent. The ssh agent will overwrite the hostname to be `github.com` and use the ssh key stored in `~/.ssh/id_rsa_for_github`.

{% include image name="Multiple ssh account for github.jpeg" %}

### Recap
* Create ssh keys (and register these keys with Github)
* Edit ssh's `config` file to indicate what ssh key to use for a given hostname
* Modify the remote url of the local Github repository to use the hostname specified in the ssh `config` file.
* Push the code to Github.

## References
* [Original Gist](https://gist.github.com/jexchan/2351996)
* [Github's article to generate new ssh key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
