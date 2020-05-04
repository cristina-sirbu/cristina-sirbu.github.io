---
layout: post
title:  "Welcome to Github Pages"
date:   2020-05-04 06:00:00
categories: github-pages
---

In order to be like people you admire you should do what they are doing. In my case, I was looking at highly skilled professional people in my field of expertise and every one of them is spreading their knowledge through a blog. Also, people say that you can’t prove that you know something unless you try to teach somebody else what you have learned. So what a better opportunity to share knowledge than through a blog.

I didn’t know how and where to start. Everything I wanted is to keep it simple, but also highly customizable. So I chose Github pages to host my blog. 

1. Create a Github repository 

    ![Screenshot1](/assets/Screenshot_1.png)

    * Go to https://github.com/new
    * Give a name to your repository.(it should match the following pattern: username.github.io where username is your Github username)
    * Make it public.
    * Check the checkbox to Initialize this repository with a README
    * Click Create Repository.

2. Enable Github Pages. 

    * Go to the Settings menu. 
    ![Screenshot2](/assets/Screenshot_2.png)
    * Scroll down to the Github Pages settings. 
    * Choose the branch from where Github will serve your blog.
    ![Screenshot3](/assets/Screenshot_3.png)
    * Choose a theme. Remember the name of the theme you choose.

Congratulations! Now you have a blog hosted at www.username.github.io.
 
But this was not enough for me. I wanted to be able to customize the theme. I wanted to make it more personal. And this is where the fun has begun. 

I went to https://github.com/pages-themes and looked for the theme I chose. I started copying stuff from the repository of my theme to mine. I copied the _layouts, _sass, assets directories along with its content. I also copied Gemfile, jekyll-theme-slate.gemspec and index.html. Now you should be able to update the theme in any possible way you want. But in order to see the changes, I had to make a push to the master branch. This doesn’t sound good, so I looked for possibilities to serve the webpage locally. This meant a lot of tools to install (Ruby, Gemfiles, Jekyll) and I wasn’t happy about it. Since we are in 2020 and we have Docker, why not using it?!

To build the project run:
```
export JEKYLL_VERSION=3.5
docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:$JEKYLL_VERSION jekyll build
```
*Note*: I am using Jekyll 3.5. If you want a different version please change the value of the JEKYLL_VERSION variable.

To run the project:
```
docker run --name newblog --volume="$PWD:/srv/jekyll" -p 3000:4000 -it jekyll/jekyll:$JEKYLL_VERSION jekyll serve --watch --drafts
```
Then go to localhost:3000 in your preferred browser.

Now the sky is the limit. You can customize your blog any way you like.

I’m hopping that this tutorial proved to you that creating a blog that looks professional is not as hard and scary as it sounds like. And maybe you might consider in the future starting your own.

