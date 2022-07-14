---
title: "Hosting a Personal Website With SvelteKit on Github Pages"
author: Bradley Leonard
date: "2022-04-02"
description: A tutorial on how to host a personal website made with SvelteKit on Github Pages based on my experience re-vamping Technickel.dev.
tags:
  - svelte
  - sveltekit
  - web
  - github
  - github pages
  - tutorial
---

# {title}

{date} - {author}
<br>

## A personal website using SvelteKit and Github Pages

Everyone should have a personal website. If you don't, what's stopping you? Since our personal website is simple, Github Pages is the perfect candidate to host on.

## SvelteKit

SvelteKit is a framework made to allow web applications to be built in Svelte. Using filesystem-based routing and a number of other modern web development features like server-side rendering (SSR), you can easily put together any website you can dream of! I am personally a HUGE fan of SvelteKit and have been using it for nearly every new project I make!

## Github Pages

Github Pages is a service hosted by Microsoft's Github that allows users to host websites directly from their repository. In a nut shell, this means you don't need to worry about the infrastructure needed to run your website, everything is all handled for you by Github. Another benefit of the service is that it can be set up to deploy automatically allowing you to also forget about complex CI / CD devops pipelines.

<br><br>

## Github Actions breakdown

```
name: build-and-push-static-website
on:
  push:
    branches:
      - main
jobs:
  build-to-gh-pages-branch:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v2
      - name: Install and Build 🔧
        run: |
          rm -rf node_modules && yarn install --frozen-lockfile
          yarn build
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4.2.2
        with:
          branch: gh-pages # The branch the action should deploy to
          folder: build # The folder the action should deploy
```

<br>

Let's break the Github Action down into it's individual steps to figure out what is going on:

- _name_ - the name given to the Action. It will display in the Github repository in your workflow list.
- _on_ - defines when to run this workflow. There are a number of triggers you can respond to. In this instance, we listen for a push on the main branch as defined by the nested lines. This allows Github to know to run this workflow as soon as we push code to the main branch.
- _jobs_ - this is the breakdown of tasks that need to be performed. You can have a number of jobs running in parallel. In our case we have a pretty simple task, build the static site and push it to another branch. Therefore, we can get away with using only one job which we named _build-to-gh-pages-branch_.
- _runs-on_ - Github is running our workflow on hardware somewhere in the world, so we have to define what we actually want to run it on. Our build process works well on ubuntu and so we will use that.
- _steps_ - we have 3 steps for this particular job. The first step will involve getting the source code, the second building the static website from the code and the third pushing the built code to another branch.
- _Step 1_ - we named step 1 Checkout because we are using it to get the source code from Github. We are introduced to the _uses_ command, which allows us to make use of Actions that other people have written to do certain things. _actions/checkout@v2_ is an Action made by Github that gets the source code of the repository that you're running in.
- _Step 2_ - this is the meat of the workflow for our purpose. We need to to get the dependencies for our code and then build it. For our cause, we need to build a static SvelteKit website using the static adapter using the _run_ command to similar to how console commands are executed. We need to first install all of the javascript dependencies with node. At the same time, we also remove the old node*modules to ensure a clean build and so that no unexpecte problems arise. We have aliased the \_svelte-kit build* command to use the _yarn build_ for simplicty which will take care of actually building the website into the files we need. As a side note, the | character on the run line is telling YAML that the indentation is a multi-line text.
- _Step 3_ - for the last step we want to push the built code to the branch that Github Pages can pick up. For this we are going to use the _JamesIves/github-pages-deploy-action@v4.2.2_ Action as it takes care of all the nuances of pushing to a branch. First we tell it what branch to push to, which in our case is going to be called _gh-pages_. Then we also tell it what files to push. In the previous step, Sveltekit built the static website to the build folder, so that's where we will have the Action look.
  <br><br>

Voila! This short and sweet file should be all we need to automatically get the code we push from main to live on our Github Pages website.

<br>

## Custom domain with Github Pages

![SVG celebrating Canada Day](/blog/canada_day_svg.png "Canada Day SVG")

<div class="text-center">

_SVG celebrating Canada Day_

</div><br>

## How to make your own SVGs

## Recap
