---
title: "Setting up Jekyll and Github Pages"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Website
tags:
  - Jekyll
  - GitHub
  - How-To
---

As I mentioned before, it has been a long time to-do item for me to get a blog/website up and running. I finally pulled the trigger and this is a result. This is the down a dirty guide..

* TOC
{:toc}

## Step 1: Select hosting

For me - and many others, this should be simple. [GitHub][GitHub] offers a free service for static pages. [Github Pages][GitHub-pages] - free hosting based off your public or private repo.

[Github Pages][GitHub-pages] has some good documentation on setting up the repo - for me, I wanted this to be my github user site, so I created the repo ```jakevis.github.io```, and enabled pages on this site. I also associated it my ```jv.ag``` domain, and set it to require https.

![GitHub Pages Settings](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/1.png)

If you are using a custom domain name, you do need to setup a cname, alias or apex record. For me, this was an alias record - and you simply point it to your default github pages site (so for me ```jakevis.github.io```).

## Step 2: Getting Jekyll running

My day to day machine is Windows 10 - so these instruction as for that. If your using Mac or Linux, changes are you have ruby allready installed, and can skip the first bit, or gem is avalible for your package manager.

For windows - go to [https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/) and download the the recommended version (with devkit).

![Ruby Download](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/2.png)

Follow the bouncing ball for installation once downloaded.

Once installed ```gem``` should be avalible to you on your command line. Verify with:

```gem --version```

![gem version](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/3.png)

Now install bundler and jekyll.

```gem install bundler jekyll```

This may take some time as it fetches what it needs from the interwebs.

![gem install](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/4.png)

## Step 3: Select your Theme and run locally.

So with luck you allready have an idea of what theme you intent to use. [Github Pages][GitHub-Pages] has a bunch that are supported by default (https://pages.github.com/themes/), you could build you own... or do what I did and find another one on the interwebs that you like.

The process is somewhat the same for all, but do check the documentation of the theme as to file layout and required files. 

My selection was this theme: [https://github.com/mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes); and luckly they also provide a sample site using said theme: [https://github.com/mmistakes/mm-github-pages-starter](https://github.com/mmistakes/mm-github-pages-starter)

To get up and running I downloaded this starter code, and committed it to my repo. You can use github desktop or whatever your preference is for handing git.
```powershell
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/jakevis/jakevis.github.io.git
git push origin master
```

Next I customized my Gem file. As I wanted to build and test the site locally, and while on the road without pushing everything to the site live. My Gem file ended up like this:

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem 'jekyll-include-cache'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
gem 'wdm', '~> 0.1.0' if Gem.win_platform?
```

```gem "github-pages", group: :jekyll_plugins```: is used to tell jekyll to use github pages (for theming ect)

```gem 'jekyll-include-cache'```: Needed by this theme

```gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]```: allows me to set my timezone and have it work correctly

```gem 'wdm', '~> 0.1.0' if Gem.win_platform?```: Removes a depandacy check - speeds the local build up a tad.

Next we should customize the ```_config.yml``` file a tad. Follow the bouncing ball, most of it just needs personalization. I did add bing verification to mine, as well as google analytics; and a link to LinkedIn. Here is mine for reference, but refer to the master theme for all options: [https://github.com/mmistakes/minimal-mistakes/blob/master/_config.yml](https://github.com/mmistakes/minimal-mistakes/blob/master/_config.yml)

Note I have shortened the description felid to make it render correctly. If you change ```:``` to ```>-``` you can the enter multiple lines of text.

```yaml
title: Jakes Site of Random Musings
email: jakevis@outlook.com
description: 
url: https://jv.ag
twitter_username: jakevis_
github_username: jakevis
minimal_mistakes_skin: dark
search: true
search_full_content: true
logo: #""
repository: "jakevis/jakevis.github.io"


# Build settings
markdown: kramdown
remote_theme: mmistakes/minimal-mistakes
# Outputting
permalink: /:categories/:title/
paginate: 5 # amount of posts to show
paginate_path: /page:num/
timezone: America/Los_Angeles

bing_site_verification: E777B921F80745027DAA711311DEFC0E

include:
  - _pages

# Exclude from processing.
# The following items will not be processed, by default. Create a custom list
# to override the default setting.
# exclude:
#   - Gemfile
#   - Gemfile.lock
#   - node_modules
#   - vendor/bundle/
#   - vendor/cache/
#   - vendor/gems/
#   - vendor/ruby/


# Plugins (previously gems:)
plugins:
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-gist
  - jekyll-feed
  - jemoji
  - jekyll-include-cache

author:
  name   : "JakeVis"
  avatar : "/assets/images/bio-photo.jpg"
  bio    : "Jack of all, master of **none**"
  location: "Seattle, WA, USA"
  links:
    - label: "Website"
      icon: "fas fa-fw fa-link"
      url: "https://jv.ag"
    - label: "LinkedIn"
      icon: "fab fa-fw fa-linkedin"
      url: "https://www.linkedin.com/in/jakevisser/"
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      url: "https://twitter.com/jakevis_"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/jakevis"
    #- label: "Instagram"
    #  icon: "fab fa-fw fa-instagram"
    #  url: "https://instagram.com/"

analytics:
  provider               : google # false (default), "google", "google-universal", "custom"
  google:
    tracking_id          : UA-145874977-1
    anonymize_ip         : false # true, false (default)

footer:
  links:
    - label: "LinkedIn"
      icon: "fab fa-fw fa-linkedin"
      url: "https://www.linkedin.com/in/jakevisser/"
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      url: "https://twitter.com/jakevis_"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/jakevis"
    #- label: "Instagram"
    #  icon: "fab fa-fw fa-instagram"
    #  url: "https://instagram.com/"

defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: false
      comments: false
      share: false
      related: true
  # _pages
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true

category_archive:
  type: liquid
  path: /categories/
tag_archive:
  type: liquid
  path: /tags/
```

Next is to get this running locally. For this, change into your repo's directory and execute:

```bundler update```

This should pull down all the required packages and set everything up.

![bundler update](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/5.png)

Once done it should conclude with:
```Bundle updated!```

If so, your now right to run it.. to get going I use:

```bundle exec jekyll serve --drafts --incremental```

![exec](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/6.png)

Once running you should be able to go to [http://127.0.0.1:4000/](http://127.0.0.1:4000/) and see your page. 

```--incremental```: This is experimental still, but speeds up rebuild times
```--drafts```: This enabled rendering a ```_drafts``` folder. When published to github, this folder is ignored; so its handy to put unfinished work in here prior to go-live.

## Step 4: Finishing up

So if your site runs localy; it should run on github too. Just commit and push your changes and you should be set. If you screw something up and it fails to build... github will email you:

![GH Email](../../assets/images/2019-08-17-Setting-up-Jekyll-and-Github-Pages/7.png)

In my case I tried to configure the ```_drafts``` folder to be a submodule (in a private repo).. for the record this isnt supported.


[GitHub]: https://github.com
[GitHub-pages]: https://pages.github.com/