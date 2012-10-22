---
layout: post
title: "how to use octopress as to posting and deploying to heroku"
date: 2012-10-22 10:14
comments: true
categories: heroku, octopress
---

When a repo is cloned, it has a default remote called "origin" pointing to your forked repo on GitHub.
To keep track of the original repo, you need to add another remote repo named "upstream"

{% codeblock lang:bash %}
git remote add upstream https://github.com/(username)/repository_name.git
git fetch upstream
git merge upstream/master
{% endcodeblock %}

Those above command will allow you to synchronize the original octopress code.

Now, it's time to post a new writing on octopress

{% codeblock lang:bash %}
rake new_post["title_here"]
# if you are using zsh, otherwise you will see this error message "zsh: no matches found: new_post["title_here"]"
rake new_post\["title_here"\]
{% endcodeblock %}

This command will set the default repository to be effected by push and fetch command.

{% codeblock lang:bash %}
# Set heroku to be the default remote for push/fetch
git config branch.master.remote heroku
{% endcodeblock %}

The below commands will commit the modified codes and push to heroku repository so that heroku can recognize your posting.

{% codeblock lang:bash %}
rake generate
git add .
git commit -m 'commit message'
git push heroku master
{% endcodeblock %}
