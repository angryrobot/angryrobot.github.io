---
layout: post
title: 'How To Create A Jekyll Blog'
date: '2019-02-07 01:25:00'
categories: tutorials
---

#### Table of Contents

1. Getting started
2. Installing Ruby & Rails
3. Installing RVM (Optional)
4. Deploying to Github Pages
5. Conclusion

#### Getting Started

Jekyll is a command line based tool used to create static websites and blogs. 

#### Installing Ruby and Rails

**Install Ruby:**

Ubuntu & Debian

```bash
$ sudo apt install ruby
```

Arch Linux

```bash
$ sudo pacman -S ruby
```

Centos / RHEL

```bash
$ sudo yum install ruby
```

**Install Rails:**

```bash
$ gem install rails
```

or you can use RVM (Ruby Version Manager)

**Install RVM:** (Optional)

Full instructions here: https://rvm.io/

**Install GPG keys:**

```bash
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

```bash
\curl -sSL https://get.rvm.io | bash -s stable
```

For installing RVM with default Ruby and Rails in one command, run:

```bash
\curl -sSL https://get.rvm.io | bash -s stable --rails
```

**Install Jekyll:**

```bash
$ gem install jekyll
```

**Install Bundler:**

*Bundler provides a consistent environment for Ruby projects by tracking and installing the exact gems and versions that are needed.*

*Bundler is an exit from dependency hell, and ensures that the gems you need are present in development, staging, and production. Starting work on a project is as simple as `bundle install`.*

```bash
$ gem install bundler
```

**Create a Jekyll Site:**

```bash
$ jekyll new blog
```

**Change Directory:**

```bash
$ cd blog
```

**Build the Site:**

```bash
$ bundle exec jekyll serve
```

When the site is finished building navagite to http://localhost:4000 in your web browser. If everything worked you should see your new jekyll blog. Now lets take a look at the file structure:

```bash
|-- _config.yml
|-- _posts/
|-- _site/
|-- 404.html
|-- about.md
|-- Gemfile
|-- Gemfile.lock
|_  index.md
```

`_config.yml` - The configuration settings for your site

`Gemfile` - The list of Gem dependencies

`Gemfile.lock` - List of Gems (packages for your project)

`_posts` - Directory for blog posts

`_site` - Directory where files are written

`404.html` - Error page

`about.md` - About page

`index.md` - Home page

**Customizing:**

Notice that our site is already styled. This is because it uses a theme named Minima. The theme’s files were installed when you installed Jekyll. To override the theme’s styles, we need to copy the files to our project folder. You can see the file path of the theme by running the command `bundle show minima`.

To get started customizing the site, copy all of the folders inside the minima directory to the project directory. Next, remove the reference to `gem "minima", "~> 2.0"` from the `Gemfile`, and remove `theme: minima` from `_config.yml`. This removes the Gem theme and lets us use our own theme. Restart the server to register the changes we made.

**Posts**

The ```_posts``` folder contains all the blog posts. To create a new post simply create a file with the format ```yyyy-mm-dd-post-title.markdown```. Now add the required front matter to the file:

```bash
---
layout: post
title:  "Build a Jekyll Blog"
date:   2019-02-07 01:25:44
author: Joe Designer
permalink: blog/build-a-jekyll-blog.html
---
```

**Deploying**:

We will be using [GitHub Pages](https://pages.github.com/) to host the new site. You will need a [GitHub](https://github.com/) account if you don't already have one and install ```git```.  Now go to your GitHub dashboard and create a new repository and name it ```username.github.io```. Change ```username``` to your username.

Open a terminal window and go to the root of your project and initialize git:

```bash
$ git init
```

Add all of the files:

```bash
$ git add --all
```

Save the changes:

```bash
$ git commit -m "my first commit"
```

Set up the remote repository to push to:

```bash
$ git remote add origin https://github.com/username/repo_name.git
```

The url will be replaced with the url you were given when you created the repository. Finally, push the changes to the remote repository:

```bash
$ git push -u origin master
```

You can now see your site online at [[http://username.github.io](http://username.github.io/)].