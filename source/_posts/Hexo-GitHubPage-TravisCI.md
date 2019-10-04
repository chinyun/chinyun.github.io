---
title: Hexo+GitHubPage+TravisCI
date: 2019-10-04 14:50:40
tags:
---

## Overview
 1. About how to use [Hexo](https://hexo.io/) build up my blog and deploy on [GitHub Pages]().
 2. How to use [Travis](https://travis-ci.com/) CI to set up automatically deployment.

## Why use Hexo
 1. run on Node.js
 2. fast build up
 3. many plugins and big community

## Start

### Prerequisites

1. npm & node.js
2. git

### Installing

Install Hexo
```
npm install -g hexo-cli
```

Add hexo folder, cd into it and install packages through npm
```
hexo init <folder>
cd <folder>
npm install
```
>Notes: the default theme 'Landscape' has an unknown variable at README.md line 77. It would lead to build failed. So need to delete the README.md file of the 'Landscape' theme. Or comment out that line. (2019.10.04)

### Setting

Edit the `_config.yml` file 
```
...
# Site
title: <YOUR BLOG NAME>
subtitle: <BLOG SUBTITLE>
description:
keywords:
author: <YOUR NAME>
language: zh-tw  //正體中文
timezone:

# URL
url: https://<username>.github.io/<repo-name>/
root: /<repo-name>/
...
```
> Notes: It is essensial to set **`root`** to **`/<repo-name>/ `** otherwise it could led to CSS file generating failed.

More details: Hexo document [Configuration](https://hexo.io/docs/configuration.html)

### Deployment

In official document tutorial, it recommends to use `Travis CI` to deploy `Github Pages`. It is free for open source repository, meaning your repository’s master branch has to be public.

#### Two options of GitHub Pages Url Choosing

1. Create a repo named `username.github.io`, where username is your username on GitHub. 

2. Create a repo named anything. for example: `myblog`.

The difference between these two options is that a user has only one site of url `https://<username>.github.io`, while you can have mutiple sites with the url `https://<username>.github.io/<repo-name>/`.


#### Push hexo files to Github

1. Push the files of your Hexo folder to the repository. 

>Notes: The public/ folder is not (and should not be) uploaded by default, make sure the `.gitignore` file contains `public/` line.

#### Add Travis CI to GitHub account

1. Go to Applications settings, configure Travis CI to have access to the repo. You’ll be redirected to Travis page.

2. On a new tab, generate a new token with repo scopes. Note down the token value.

3. On the Travis page, go to your repo’s setting. Under Environment Variables, put GH_TOKEN as name and paste the token onto value. Click Add to save it.

4. Add `.travis.yml` file to your repo (alongside `_config.yml` & `package.json`) with the following content:
```
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```

#### Deploy

1. To create a new post, youn can add `<title>.md` file in the `source/_posts` folder by running following command:
```
hexo new [layout] <title>
``` 
> Notes: `post` is the default layout. To change the default layout by editing the default_layout setting in `_config.yml`.

2. You can monitor the building process at Travis CI dashboard. And add the image of build/passing by click it to copy the markdown script and paste into your `README.md` file in hexo folder.

3. Once Travis CI finish the deployment, the generated pages can be found in the `gh-pages` branch of your repository

4. In your GitHub repo’s setting, navigate to “GitHub Pages” section and make sure that the Source is pointed to `gh-pages` branch.

5. Now you can check the webpage at the url you set.

## Reference

[Hexo Docs](https://hexo.io/docs/configuration.html)
[GitHub Pages](https://help.github.com/en/articles/getting-started-with-github-pages)
[Travis CI Docs](https://docs.travis-ci.com/user/tutorial/)

