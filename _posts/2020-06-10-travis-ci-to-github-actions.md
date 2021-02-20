---
layout: post
current: post
cover:  assets/images/2020/Jun/github-actions.png
navigation: True
title: Moving from Travis CI to GitHub Actions
date: 2020-06-10 16:00:00 +7
tags: dev
class: post-template
subclass: 'post dev'
author: vinh
---

As you may already know, for this website, I'm using Jekyll with the help with some automations from Travis CI for the past two years. Because that's what the theme developer had recommended. But recently, Travis has decided to combine travis-ci.org and travis-ci.com and I had some problems with the migration to travis-ci.com. Authentication seemed never work. After many frustrating attempts to fix it, I decided to move to something else. There are so many options, but since I'm using GitHub, why not use the in-house solution, GitHub Actions.

# Let's get started...
In my limited knowledge of GitHub Actions, I know there will have to be a *config file*, just like Travis. It should be located at **.github/workflows/**. The file should be in YAML format and the name shouldn't matter. In my case, it's *main.yml*

If you create it from the GitHub user interface, the sample file will look like this:

```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
push:
    branches: [ master ]
pull_request:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
# This workflow contains a single job called "build"
build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
    - uses: actions/checkout@v2

    # Runs a single command using the runners shell
    - name: Run a one-line script
    run: echo Hello, world!

    # Runs a set of commands using the runners shell
    - name: Run a multi-line script
    run: |
        echo Add other actions to build,
        echo test, and deploy your project.
```

It's quite easy to understand. But the theme I'm using is deeply written for Travis integration. At first, I didn't know it so I get a lot of issues when copying and pasting from Travis config file. The tasks here are very simple, when there is a push action to a branch on GitHub, combine and build production files for the website and push it back to master branch. For hours, I had stuck at anthentication steps. I had tried some available actions in the marketplace but it doesn't work like I intend. Then, I checked Jekyll theme source, it was deeply written for Travis and I have to make some changes to it so that I can use it with GitHub Actions. And ta-da, after dozens of failed build, GitHub Actions finally showed green tick.