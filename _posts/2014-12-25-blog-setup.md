---
layout: post
title: Blog Setup 
---


<div class="message">
Howdy! This post lists the steps involved in setting up this blog
</div>

I forked this beautiful blog template from [Hyde](https://github.com/poole/hyde). Like mentioned in the README for the project, we should not be using the forked `gh-pages` branch as it contains their Google analytics tracking code and other specific information. For the same reason, I deleted their original `gh-pages` branch so all their speicific changes are lost and created a branch with the same name `gh-pages` so that Github can create the pages for me, but it will have the default configuration rather than `Hyde` hosted page specific configuration.

To delete remote branch,

`git remote origin --delete gh-pages`

Followed by

`git checkout -b gh-pages`

Followed by changes to `_config.yml`, then

`git push remote origin`

To push the changes done in gh-pages branch to master

`git checkout master`

followed by

`git merge gh-pages`


Hope this helps! 
