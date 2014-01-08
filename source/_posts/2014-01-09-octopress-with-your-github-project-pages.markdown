---
layout: post
title: "Octopress with your github project pages"
date: 2014-01-09 02:16:19 +0900
comments: true
categories: Tips
---
You can build your blog on your own github **project** repository using [Octopress](http://octopress.org). Here's the [instruction](http://octopress.org/docs/setup/) how to set it up. While you'd find most of the things there, there's a thing you need to take care of if you intend to get it done with your **project** pages (gh-pages) rather than with the **user** pages (\<username\>.github.io).

#### Clone the Octopress and install dependencies
(Ruby >1.9.3 required)
```
$ git clone https://github.com/imathis/octopress.git
$ cd octopress
$ sudo gem install bundler
$ bundle install
```
#### Install a theme
```
$ rake install
```
Check [here](http://opthemes.com/) if you want to find some additional themes instead of the default one.

#### Deploy to github project pages
```
$ rake setup_github_pages
```
You'll be asked for the url of the github repository. Just enter `https://github.com/<username>/<project>.git`

(If you'd tried deploying to your `<username>.github.io`, the following two steps would have NOT been required.)

#### Add remote origin
```
$ git remote add origin https://github.com/<username>/<project>.git
$ git config branch.master.remote origin
```
#### Correct things manually
Open `_config.yml`. You might see the string `/github` for the properties `url`, `subscribe_rss`, `root`, `destination`. E.g. `url: http://<username>.github.io//github`.

Change them to the project name of your actual repository. Here's mine (for `https://github.com/jungkees/log.git`):
```
url: http://jungkees.github.io/log
subscribe_rss: /log/atom.xml
root: /log
destination: public/log
```
Open `config.rb` and do the same (`/github` to `/<project>`).
I got this after changing them:
```
http_path = "/log/"
http_images_path = "/log/images"
http_fonts_path = "/log/fonts"
css_dir = "public/log/stylesheets"
```
Open `Rakefile` and change `public_dir = "public/github"` to `public_dir = "public/<project>"`.

Before you go to the next step, find the directory `public/github` and change it to `public/<project>`. Now it seems you are ready to generate the blog.

#### Generate and deploy
Run these commands:
```
$ rake generate
$ rake deploy
```
The generate command creates the blog at `public/<project>` and deploy command move the blog to `_deploy` directory and push the content to github. (`gh-pages` branch is used automatically.)

### Push the source code
Lastly, push the entire blog assets to github. With the **project** pages, the branch is `master` by default. Thus, it will be like:
```
$ git add .
$ git commit -m "Update the source"
$ git push origin master
```
Alright. You've done it. Here's the Octopress [tutorial](http://octopress.org/docs/blogging/) for the blog posts. Enjoy your posting in beautiful markdown!
