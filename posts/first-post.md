---
title: Blogging with Ruhoh
description: Updated '2013-09-30'
date: '2013-06-15'
tags: ["blog", "productivity"]
categories: software
---
It has been three years since I started [my first static blog](http://lessvariance.herokuapp.com/). My apology for the lack of postings. It has been a long difficult journey with no sight of the tide turning. I can only say that I failed to get my priorities straight. As the `ruhoh` blogging platform is very similar to `jekyll`, I decided to give it a try. I'm also fond of using `markdown` formatted text given its flexibility, and static blogs generally support that. From now on, I'm going to start fresh here.

Ruhoh comes with the core feature sets I need. Its building blocks are well-abstracted. At the time of writing, I'm using `ruhoh version 2.1`. The Ruhoh documentation has been helpful, and I was able to get up to speed thanks to another excellent [writeup](http://blog.coolaj86.com/articles/hosting-your-blog-on-ruhoh-com.html). Although there is no need to repeat anything here, I'd love to be able to understand the process better and document some essential workflow here for my future reference.

To get started, clone the blog scaffold from this [github repository](https://github.com/ruhoh/blog). 

```
$ git clone git://github.com/ruhoh/blog.git blog
```

To enable adding our own contents later on, we'll need to remove the reference to the Ruhoh origin before pointing it to our own. Assuming we have already set up a github repository with `USERNAME/ruhoh-blog.git`, and the blog directory on the local machine has its name set to `username.github.io`, we do:

```
$ git remote rm origin
$ git remote add origin git@github.com:USERNAME/ruhoh-blog.git
```

Make sure we have at least `ruby 1.9.2+` and check if bundler exists: `$ bundle -v`. Bundler is used to manage different versions of Ruhoh. If the bundler is not found, install it using `$ gem install bundler`. Inside the working blog directory, install the Ruhoh gem using bundler. 

```
$ bundle install
```

Once the bundle is installed, we may run the blog in *preview* mode. Run in the terminal:

```
$ bundle exec rackup -p 9292
```

And open [http://localhost:9292](http://localhost:9292) in the browser. 


### Customizing Ruhoh
The blog theme is the part where I had spent the majority of my time. Coming from the software engineering background, I have a lot to learn from  the front end people. It was only after many attempts at testing and previewing on local machine, I was able to pin down to individual selectors and rules to modify them accordingly. 

The theme that works with Ruhoh out of the box is the popular Twitter Bootstrap. Looking inside `/twitter/styesheets/bootstrap.min.css` can be quite daunting due to the long unwrapped lines. After trying to work with it, I found it was too time consuming if our goal was an entirely different site design. There is a faster way to customize it [online](http://getbootstrap.com/2.3.2/customize.html) without worrying about its nitty-gritty, but I didn't take a plunge into it as I was rather interested in learning CSS from the ground up. It is therefore best to leave the default theme alone unless you plan only for a minor modification.

Instead, we can write a new CSS to override the default CSS while still leaving the task of providing the most basic styling needs to the default bootstrap CSS. Place this new CSS file alongside `bootstrap.min.css`, both of which will be referenced from our HTML page `/twitter/layout/defaults.html`. Check out a few CSS samples that give looks to our liking and use them to help us write this new stylesheet.

To facilitate proper display and resize on any mobile devices, [Skeleton](http://www.getskeleton.com/) provides a set of CSS files for responsive design. The grid system helps to create a layout with any number of columns we choose.

There are two `.yml` files which are [YAML](http://en.wikipedia.org/wiki/YAML) documents. These configuration files can be edited to include any user-specific data for putting together a unique blog.

- `config.yml` let us specify the production url, blog theme, format of post permalink, and paginator preference. It is recommended that we set the post permalink to `/posts/:title/` in case we break it when we have to change post categories. Widgets such as google analytics and disqus comments can be set up easily in this file without having to hardcode the default HTML blog layout.

- `data.yml` let us set the title of our blog, tagline (if there is one), as well as author properties. Furthermore, we can specify which pages to display on the top or sidebar navigation.


### Creating Blog Posts

We can create a new post with Ruhoh by running:

	$ bundle exec ruhoh posts new "Another Post"

	Using theme: "twitter"
	New posts:
  		> posts/another-post.md

  	# Bring up the Sublime editor to start writing post
	$ subl posts/another-post.md

Notice that every post has the same template with the top metadata included.

	---
	title: Blogging with Ruhoh
	description: 
	date: '2013-06-15'
	tags: ["blog", "productivity"]
	categories: software
	---

	... Type away ...


### Deploying the blog on Github Pages

Once we are done with the posts, we are ready to publish them. This step basically consists of pushing the compiled blog to Github for blog hosting, and pushing the blog files like we usually do with any other repository. I'm using the free Github Pages to host this blog. There are other options like Heroku which I've used before, and Amazon S3. 
	
	# If the branch doesn't exist, run this one-time command to create a new branch with the name blog: $ git checkout -b blog.
	# Otherwise, just check out to the blog branch since we need to compile the blog from here
	$ git checkout blog

	# Compile the blog in production mode. This creates a new compiled/ folder inside our working blog directory
	$ bundle exec ruhoh compile

	# Exclude the compiled folder and lock files
	$ echo "compiled" >> .gitignore

	$ echo "*.lock" >> .gitignore

	$ echo ".DS_Store" >> .gitignore

	# On Mac OS, there is this extra hidden file .DS_Store we would like to exclude
	$ find . _name .DS_Store -print0 | xargs -0 git rm --ignore-unmatch

	$ git add .

	$ git commit -m 'message'

	# Make sure everything has been added and commited before switching branch
	$ git status

	# Now, we are ready to push the compiled blog
	$ git checkout master

	# Copy files under compiled/ to the top directory
	$ rsync -a compiled/ ./

	# Remove compiled/ dir
	$ rm -rf compiled/

	$ git add .

	$ git commit -m 'message'

	$ git push origin master

Note that the blog `username.github.io` has two branches: master and blog. We have already pushed the compiled blog to the `master` branch. Now, we are going to push the blog files to the `blog` branch.

	$ git checkout blog

	$ git add .

	$ git commit -m 'message'

	$ git push origin


### Ruhoh Update
With periodical updates to Ruhoh about once every few months, we may want to get the latest Ruhoh gem.  


Jennifers-MacBook-Pro:estimate.github.io leecarrot$ bundle exec ruhoh server 9292
Using theme: "twitter"
/usr/local/Cellar/ruby/1.9.3-p0/lib/ruby/gems/1.9.1/gems/ruhoh-2.5/lib/ruhoh/parse.rb:37:in `gsub': invalid byte sequence in UTF-8 (ArgumentError)