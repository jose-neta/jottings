---
layout: post
title:  "Vi plugin manager"
date:   2020-04-01 15:00:00 +0000
categories:
tags: vim
---
## TL;DR

From vim 8.0 onward you don't need a package manager. You just need to create two folders

`mkdir -p ~/.vim/pack/vendor/{start,color}`

Inside `start` folder is where you place the plugins you need and inside `color` folder is where you place the color schemes. 

And for spelling it is only a matter of one more line inside `.vimrc` file

## Setup

Start by creating the following _scafollding_

```sh
mkdir -p ~/.vim/pack/vendor/{start,color}
```

And that's all it takes, you are now ready to install your favorite _plugins_ and color schemes.

## Install some plugins

### Nerdtree

To install `nerdtree` simply clone its code into `start` folder

```sh
git clone https://github.com/preservim/nerdtree.git ~/.vim/pack/vendor/start/nerdtree
```

Some plugins require some extra steps after install, and `nerdtree` is no exception

```sh
vim -u NONE -c "helptags ~/.vim/pack/vendor/start/nerdtree/doc" -c q
```

One extra step is required for you to have `nerdtree` available every time you start `vi`. Place the following lines onto `~/.vimrc`

```
map <leader>n :NERDTreeToggle<CR>
map <C-n> :NERDTreeToggle<CR>
```

The first line allow you to type `backslash + n` to show up the `nerdtree` and the second one allows you to type `CTRL+n` instead. 

### Airline

`Airline` is nice and lean status bar, hence let's install it

```sh
git clone https://github.com/vim-airline/vim-airline ~/.vim/pack/vendor/start/airline
```

And that's all it takes to install `airline`, but, if you expect to see the git branch name you'll need to install `vimfugitive` first, so let's do it afterwards :)

### Fugitive

To install `fugitive plugin` as usual clone its code

```
git clone https://tpope.io/vim/fugitive.git ~/.vim/pack/vendor/start/fugitive
```

`Fugitive` plugin needs as bit of configuration, hence the following line is the extra step you need to take to correctly configure it.

```
vim -u NONE -c "helptags fugitive/doc" -c q
```

## Install a color scheme

Now it's time to install some color scheme. A color scheme is simply a `*.vim` file that you will drop into the `~/.vim/color` folder.

### iceberg

Let's install `iceberg` color scheme

```
git clone https://github.com/cocopon/iceberg.vim.git ~/.vim/pack/vendor/start/iceberg
cd ~/.vim/colors/
ln -s ~/.vim/pack/vendor/start/iceberg/colors/iceberg.vim .
```

Now lets setup`iceberg`, for that you simply need to put the following lines on `.vimrc` file

```~/.vimrc
colo iceberg
syntax on
```

And that's it. Up until now you have a color scheme and more many plugins installed and you don't touched any of those fancy `patogen` or `vundle` or whatsoever plugin/package manager as they call them.

# Install a dictionary

One more thing that comes in handy is a tool to check my spells. For that reason simply add the following line onto your `~/.vimrc`.

```~.vimrc
set spell spelllang=en_us
```

If you happen to write code or in another language, you will find yourself wanting to turn down the check spelling `en_us` from time to time, and for that `:set nospell`.

# Conclusion

It's 2020 and I still prefer to write prose or code using `vi` and with the smallest then ever `~/.vimrc` file.

```~.vimrc
map <leader>n :NERDTreeToggle<CR>
map <C-n> :NERDTreeToggle<CR>

colo iceberg
syntax on

set spell spelllang=en_us
```
