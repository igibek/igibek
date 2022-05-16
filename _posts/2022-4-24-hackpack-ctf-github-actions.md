---
layout: post
title:  "HackPack CTF: GitHub Actions Challenges"
date:   2022-4-24
description: HackPack CTF'22 challenge development process
tags: 
    - security
    - ci/cd
    - ctf
---

Since 2018, as a hackpack team, we host HackPack CTF annually. Following the tradition, this year we hosted the catch the flag challenge in April. If you have experience in hosting CTF competitions, you know that one of the hardest parts of organizing such kinds of events is to develop challenges that will be interesting for everyone. Every year, we sit down together and start brainstorming about challenges 2 to 3 months in advance before the date of the competition. This year, I developed two challenges (1) [Geet-into-actions](https://github.com/hackpack-ncsu/geet-into-action) and (2) [Geet-into-reactions](https://github.com/hackpack-ncsu/geet-into-action). As you can guess from the name the challenges are related to GitHub Actions. GitHub Actions is a CI/CD platform introduced by GitHub that gained tremendous popularity among open-source communities and developers in recent years. 
Since this was the first time when I am developing challenges related to CI/CD pipeline, the development process was quite challenging for me. In the past, I usually created web challenges with injection vulnerabilities (SQL, command), XXE, path traversal and etc. 

In this post, I want to write about the difficulties that I faced during the development process and the shortcuts I made. If you have good experience in developing CI/CD-related CTF challenges, please let me know in the comments so that I will use your approach.

In short, GitHub Actions is one of the public CI/CD platforms such as Travis-CI, Circle-CI, and others that is maintained by GitHub itself, which makes it a popular choice for projects hosted on GitHub. 

To use the GitHub Actions developers need to write a workflow configuration file under the .github/workflows folder inside the repository. The workflow configuration file is a YAML file that defines events that trigger the workflow, and jobs that are executed if certain events are fired. The jobs consist of N number of steps (aka commands) executed in order. Inside the commands, developers can use events data to perform tasks. For example, one can write a build command to be executed if the issue with the title “[build]” is created. 

The basic idea of the challenge was to subvert the execution flow of the CI/CD pipeline by injecting code into the job through the creation of the issue. While the first challenge's attack is straightforward, for the second challenge you need to have to create an issue that follows a certain format. You can access both challenges by following links: geet-into-action and geet-into-reaction.

In addition to injecting the code, attackers need to figure out how to extract secrets from the CI/CD pipeline. GitHub Actions by default masks all the secrets. Therefore you can not just print out the secrets into the console. You have to manipulate it a little bit to be able to print it out. 
During the development of the challenges, I faced mainly two problems:
1. Where I can host the CI/CD pipeline runner?
2. How I can hide the traces of the exploit from other competitors? 

For the first problem, I ended up using default hosting by GitHub instead of hosting the pipeline runner in a VM. GitHub itself does not suggest using the self-hosted environment due to security risks. Since I know that both of the challenges’ workflows contain code injection vulnerability, I do not want to run arbitrary code inside the VM, even if it is isolated from the host machine. I wanted to avoid the situations when participants destroy (un)intentionally the VM so that others will not be able to solve the challenges. To our surprise, GitHub provides unlimited runtime minutes for public repositories. Even though, we were worried that GitHub will block our repository that hosts challenges due to suspicious activities. I think, at the highest workload participants were creating 20-30 issues per minute inside the repositories that were starting the execution of the workflow. To avoid stressing the GitHub infrastructure, we on purpose set the timeout minute for the challenges to 1 and 2 minutes respectively.

For the second problem, I ended up creating another job and workflow which will be executed immediately after the initial job and workflow finish execution. The goal of this job and workflow was to clean all the traces of other participants' exploits. Since participants are using the issue to inject, others can copy the solution by just looking at others' issues. Unfortunately, GitHub does not allow the deletion of the issue, but it allows us to edit the issue. That is what the job does. It edits the body and the title of the issue as soon as the vulnerable workflow starts running. Another thing to hide is the solution itself. Developers can access the execution traces of all workflows by navigating to the Actions tab in the repository. We don’t want the situation when other participants can just read the flag (solution) by reading the execution traces of previous workflow runs. Therefore, I created a cleaner workflow that is fired after the vulnerable workflow finishes execution, which removes the workflow’s traces that triggered the cleaner workflow.

I think that is it. In general, it was quite an interesting experience to develop CTF challenges related to CI/CD pipeline. If I will have a chance next year, I would like to work on a more complicated CI/CD pipeline challenge that needs the chaining of several vulnerabilities to find the flag. 

*PS: You can access the challenges by following these links: [geet-into-action](https://github.com/hackpack-ncsu/geet-into-action) and [geet-into-reaction](https://github.com/hackpack-ncsu/geet-into-reaction), and try to solve them yourself. If you think I should have taken a different approach to creating these challenges, please let me know in the comments.*