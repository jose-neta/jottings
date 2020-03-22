---
layout: post
title:  "Using GitHub pages to blog"
date:   2020-03-22 15:10:00 +0000
categories:
---
# Install Ruby

I'll use `rbenv` so that I can have multiple `Ruby` version on the same machine and don't have to bother with permissions if I happen to work on a guest account.

```sh
brew install rbenv
rbenv init
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

Restart the shell and choose a `Ruby` version from the list bellow

```sh
rbenv install -l
```

I have chosen `2.7.0`

```sh
rbenv install 2.7.0
```

Make it global, so that every time you type `ruby`, it is for sure you'll be using the version you happen to install

```sh
rbenv global 2.7.0
```

Give it a last check

```sh
rbenv versions
```

# The blog

After instalation of `rbenv` and a recent version of `ruby`, you're now ready to go.

```sh
mkdir blog && cd blog
bundle init
gem update
bundle add jekyll
```

That's it now you can test it locally with

```sh
bundle exec jekyll new . --force
bundle install
bundle exec jekyll serve
```

Now let's update `Gemfile` and leave it exactly as follows

```
gem "github-pages", group: :jekyll_plugins
group :jekyll_plugins do
  gem "jekyll-feed"
end
```

then add the `github-pages` gem

```sh
bundle add github-pages
```

update the bundle

```sh
bundle update
```

Test it

```sh
bundle exec jekyll serve
```

Edit your config file so that it reflects the github repo where you have placed your blog

```yml
baseurl: "/jottings"
```

# Customize jekyll theme

Now it's time to fine tune the theme. We are using the default theme. To find it `bundle info --path minima)` will expose its path

Every layout or parcial you are willing to customize must be included in your blog root folder. In my case I just needed to copy `_layouts/home.hml` from path above and some parcial `_includes/footer.html` and `_includes/social.html` and edit them accordingly to my taste.



Have fun!

# Reference

- [GitHub Pages docs](https://help.github.com/en/github/working-with-github-pages)
- [Jekyll theme docs](https://jekyllrb.com/docs/themes/)
