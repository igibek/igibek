---
title: "Github now offers automatic security dependency fix"
tag:
- devops
- automation
- security
description: "Github is testing automatic security fix by updating dependency tree."
---

Today I noticed an interesting feature from Github. They are testing out automatic fix of vulnerable dependency in your repository. Github will automatically create a branch with updated patch number of vulnerable dependency for you. Also, it will create a pull request for you, that you can verify and merge with the main (master) branch of the repository. If you accept to merge it, Github will delete the merged branch for you. 

Below you can see the screenshot of one of my repositories. In my opinion it is very cool feature for those who needs to keep there branch as secure as possible. Only one question remains: how stable it will be? What happens if developer do not follow semantic versioning and introduces breaking changes in a patched version?

![Github Automatic Security Fix Screenshot][screenshot]

[screenshot]: {{ site.url }}/assets/images/posts/github-automatic-security-fix.png