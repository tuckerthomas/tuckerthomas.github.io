---
layout: post
title: "Using Github Pages"
date: 2019-08-13 21:38:11 -0400
categories: [software, tutorial]
tags: [jekyll, ruby, blog, github pages]
---
### Github Pages

Github Pages hosts a static site within a Github repository and then allows Github to host the site. You can use these sites for individual projects or even as a living resume and blog (my current plan).

For more information, check Github Pages [site][github-pages].

For generating a static site, there's plenty of tools. Really, just search 'static site generator' and you'll find an abundance of tools. I decided to go with a well supported tool called [Jekyll][jekyll].

### Jekyll Setup
Jekyll was written in ruby and was rather simple to install. If you've used python and virtual-env [rbenv][rbenv] is right up your alley. It automatically manages ruby versions and switches based on a local `.ruby-version` file instead of local binaries like virtual-env.

Once you've determined if you want to install rbenv, you can follow this guide like installing ruby gems.

Next we'll install jekyll and bundle gems.
```
gem install jekyll bundler
```

And initialize a new static site 'My Blog'
```
jekyll new myblog
```

This will create a folder with everything we need inside of it.
```
cd myblog
```

Once inside, you can generate or build the site with
```
bundle exec jekyll build
```

Alternatively, if you want to host the site locally with the added benefit of hot-reloading
```
bundle exec jekyll serve
```

When building, this will generate the `_site` folder. This is where all the static files will be placed, so that we can serve them in the future.

### Putting it All Together

So, the way Github Pages works is that it pulls from the `master` branch of the user's `user.github.io` repository and serves that on a web server. Since we're using a static site generator we need to host the source/template files on another branch.

The cleanest way that I will suggest is to put the source files onto a `source` branch and then put the generated `_site` folder onto the 'master' branch.

When initializing the new jekyll site, make sure that a `.gitignore` file is created with the folder `_site` added to it. This will allow us to not track the generated site and initialize another git repo within the folder.

Basically, within the `myblog` folder
```
git init
git remote add origin https://github.com/user/repo.git
git checkout -b sources
git add *
git push origin sources
```

Then, within the `myblog/_site` folder
```
git init
git remote add origin https://github.com/user/repo.git
git add *
git push origin master
```

Anyway, once that is setup you can locally make any edits to the site on the `sources` branch and push them as needed. When you want to actually update the site on Github Pages, push the generated `_site` folder to the master branch from within the folder.

[github-pages]: https://pages.github.com/
[jekyll]: https://jekyllrb.com/
[rbenv]: https://github.com/rbenv/rbenv