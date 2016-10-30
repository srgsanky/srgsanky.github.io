---
layout: post
title:  "How to use jekyll?"
date:   2016-10-29
categories: jekyll
---
* TOC
{:toc}

## How to use jekyll to write your blog?

Follow the [Github article](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) to install and setup your jekyll site. Basically, you do the following

### One time installs
* Install `ruby`
* Install `bundler`

### Steps for every new jekyll site you create
* Create a local repository for your jekyll site.
* Create a (ruby) `Gemfile` (something that keeps track of the dependencies) and add jekyll as a dependency.
* Install jekyll by invoking `bundle install`
* Generate jekyll site files `bundle exec jekyll new . --force`
* Build your jekyll site `bundle exec jekyll serve`
* You can preview your site at `http://localhost:4000`

## FAQs

### Changes to `_config.yml` are not updated when I run `bundle exec jekyll serve`
Changes to `_config.yml` requires you to rerun `bundle exec jekyll serve`. More information can be found on the `_config.yml` file itself.

### My blog's date is different when I push the code to Github and navigate to the site.
If you included timestamp in the `date` of the post. For e.g.

```
date:   2016-10-26 23:53:00 -0700
```

In the above example, the timestamp is in `PST`. But it corresponds to `Oct 27` in `GMT`. The Github servers use `GMT`. When the Github server tries to evaluate the following from `index.html`

```
{% raw %} {{ post.date | date: "%b %-d, %Y" }} {% endraw %}
```

it will end up with `Oct 27` instead of `Oct 26`.

### How to escape characters?
* [How to escape liquid template tags?](http://stackoverflow.com/a/5866429)
* [How to escape double curly braces?](http://stackoverflow.com/a/24102537)

### Should I commit `Gemfile.lock` file?
Yes. It contains the specific versions of the dependencies that you are using.

Committing it will make sure that you are using the same version of dependencies everywhere. If you don't commit it, some machines might use different versions of dependencies that can prevent the build from succeeding.

### When I try to build the site using `bundle exec jekyll serve`, I get the following error and the styles are broken.
```
GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
```
Try the following changes

|`_config.yml`|Comment the key named `repository` using `#`.|
|`Gemfile`|Comment the line `gem "github-pages", group: :jekyll_plugins` and add/uncomment the line `gem "jekyll", "3.2.1"`|



Do not commit the above changes. If you commit them, accessing it using Github site won't work.

Note: If you change your `Gemfile`, the `Gemfile.lock` file will also be updated. So don't commit `Gemfile.lock` either when you make the above changes.

### Markdown quick reference
[Wordpress's markdown quick reference](https://en.support.wordpress.com/markdown-quick-reference/)

### How to add table of contents to a blog post?
Based on [jekyll toc markdown](http://www.seanbuscay.com/blog/jekyll-toc-markdown/), just use the following
<pre>
* TOC
{:toc}
</pre>

## References
* [Github article - settting up your github pages site locally with jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)
* [How to escape liquid template tags?](http://stackoverflow.com/a/5866429)
* [How to escape double curly braces?](http://stackoverflow.com/a/24102537)
* [Wordpress's markdown quick reference](https://en.support.wordpress.com/markdown-quick-reference/)
* [jekyll toc markdown](http://www.seanbuscay.com/blog/jekyll-toc-markdown/)
