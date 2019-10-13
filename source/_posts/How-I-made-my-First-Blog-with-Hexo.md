---
title: How I made my First Blog with Hexo
date: 2019-10-12 21:39:41
tags: hexo
categories: dev
---

# How I started all this
   For long, I have always dreamed of making some kind of "visible" product - such as web or mobile app. By progamming. For most of the times I used programming to develop better algorithms or behind-the-scene systems. I guess lack of experience with web-front is why I always had a thirst for that. Busy life, my lazziness, and groundless fear that learning front-end would be hard all combined, made me keep delaying to convert my will into action.
   
   But today, finally I took action and succeeded in creating my personal blog! From the process, I realized that there is no need to be frightened by the idea that you don't know where to start. I asked my friend who has an experience of making her personal website for advice, and learned that there are largely two frameworks for website building - Jekyll & Hexo. I had literally no idea what those were, so I started googling. I analyzed their pros and cons and decided to use Hexo to create my website. I just googled for guidelines various people posted about how they created their blog using Hexo, but later on, I realized that following the [Official Hexo documentation](https://hexo.io/docs/)  is the **clearest** solution.
   
   Here I explain the fundamentals and how-to guidelines that I have learned from my small development experience.

# What is Hexo
   There are many types of **static site generators**. Most famous ones are Jekyll - based on Ruby and Hexo - based on Node.js. Servers of static website only sends already-saved html files to clients when requested - there is no dynamic interaction taking place. Since static website doesn't need dynamic server side processing, its is super fast. Usually blogs are for displaying contents, it can be easily created as static website. 

Jekyll
  - slow build time 
  - "as you website content grows, the build process becomes significantly slower (this is the major weakness of Jekyll)"  - from https://www.techiediaries.com/jekyll-hugo-hexo/
  - (btw it says that it supports incremental build experimentally) 
  - Ruby based
  - large community

Hexo
  * fast build time (hmm...really???)
  * Node.js based
  * small community (mostly China)    

# How-to steps
## Write Blog content
1. Setup
* Follow the instructions on [Official Hexo documentation](https://hexo.io/docs/) to install node.js, hexo, and git (if you don't have them installed already)

2. Create Hexo directory
```
$ hexo init my_blog // create and initialize hexo in target folder, my_blog
$ cd my_blog
$ npm install
```
* Also, initialize this as git repo, since we will push this into git repo.

3. write contents
* _config.yml: configure settings ex) theme
* source: _posts: write your blog content
* themes: customize your theme -> there are lots of beautiful themes 
```
$ hexo new [layout] <title>
// [layout] is one of post, page, draft
// create new content
```

## GitHub Page Deployment

* Follow the official instruction here: https://hexo.io/docs/github-pages.
* Read [this](https://maologue.com/Auto-deploy-Hexo-with-Travis-CI/#Check-your-HEXO-repo-on-Travis) to understand how Travis performs automatic build & website deployment.

* Here are some things you should know but not mentioned in the instruction above.
   * [user_name].github.io must be built from the master branch. Therefore you should modify .travis.yml written in the official instruction a little bit to make it work properly. This default .travis.yml detects changes in `master` branch and if there is change, Travis build Hexo, and Travis deploys the build result into `gh-pages` branch which is used to make webpage. But, we cannot make the github page built from `gh-pages`.Thus use the configuration below!
```
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - blog-source # build blog-source branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: blog-source
  target_branch: master # if not mentioned, default is gh-pages
  local-dir: public
```
    * You might get confused because there are both travis-ci.com & travis-ci.org. Originally .com was for private repos, and .org was for public repos but Travis CI is currently migrating the .org into .com. So, you should only use .com.

    * If you want to create github blog inside github organization -> create machine github account to generate token to use for Travis. (token can only be created per users not organizations..)

# Comments
* Follow the official documentation!
* Hexo has quite some stuff (linking Travis to your repo) to do to configure auto-deployment. 
* In the near future, I will try out Jekyll as well, and see which is more convenient.