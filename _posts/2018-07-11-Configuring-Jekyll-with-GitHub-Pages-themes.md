---
layout: single
title: "Configuring Jekyll with GitHub Pages themes"
excerpt: "A guide on choosing a Jekyll theme from a wide variety of GitHub
Pages themes using just the shell with Ruby and my trusty keyboard."
author: Ryen Tang
date: 2018-07-11 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Jekyll
  - GitHub
  - Ruby
  - "Ryen Tang"
---

A few days ago, I just started deploying a static blog site using Jekyll on
GitHub Pages and you probably noticed that using `jekyll new .` will deploy
Jekyll with a default `minima` theme. Today, I'm going to demonstrate on how
you can change your Jekyll default theme for your GitHub Pages.

## GitHub supported Jekyll Themes

As you probably discovers more about Jekyll, you will find that there are alot
of themes available out there but we will only be discussing on the following
[GitHub supported themes](https://pages.github.com/themes/) below:

| Themes repository | Preview theme | Config theme key value |
| -- | -- | -- |
| [Architect](https://github.com/pages-themes/architect) | [Demo](http://pages-themes.github.io/architect) | jekyll-theme-architect |
| [Cayman](https://github.com/pages-themes/cayman) | [Demo](http://pages-themes.github.io/cayman) | jekyll-theme-cayman |
| [Dinky](https://github.com/pages-themes/dinky) | [Demo](http://pages-themes.github.io/dinky) | jekyll-theme-dinky |
| [Hacker](https://github.com/pages-themes/hacker) | [Demo](http://pages-themes.github.io/hacker) | jekyll-theme-hacker |
| [Leap day](https://github.com/pages-themes/leap-day) | [Demo](http://pages-themes.github.io/leap-day) | jekyll-theme-leap-day |
| [Merlot](https://github.com/pages-themes/merlot) | [Demo](http://pages-themes.github.io/merlot) | jekyll-theme-merlot |
| [Midnight](https://github.com/pages-themes/midnight) | [Demo](http://pages-themes.github.io/midnight) | jekyll-theme-midnight |
| [Minima](https://github.com/pages-themes/minima) | [Demo](http://pages-themes.github.io/minima) | minima |
| [Minimal](https://github.com/pages-themes/minimal) | [Demo](http://pages-themes.github.io/minimal) | jekyll-theme-minimal |
| [Modernist](https://github.com/pages-themes/modernist) | [Demo](http://pages-themes.github.io/modernist) | jekyll-theme-modernist |
| [Slate](https://github.com/pages-themes/slate) | [Demo](http://pages-themes.github.io/slate) | jekyll-theme-slate |
| [Tactile](https://github.com/pages-themes/tactile) | [Demo](http://pages-themes.github.io/tactile) | jekyll-theme-tactile |
| [Time machine](https://github.com/pages-themes/time-machine) | [Demo](http://pages-themes.github.io/time-machine) | jekyll-theme-time-machine |

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Setting up Jekyll for GitHub Pages themes 

After you had executed `jeykll new .` in your local repository path to create a
new static site, you can use the following Ruby script to modify Gemfile to use
`github-pages` gem to bootstrap dependencies for setting up and maintaining a
local Jekyll environment in sync with GitHub Pages.

```ruby
text = File.read('Gemfile')
File.write('Gemfile', text.gsub(/gem\ \"jekyll\"/, '#gem \"jekyll\"'))
text = File.read('Gemfile')
File.write('Gemfile', text.gsub(/#gem\ \"github-pages\"/, 'gem \"github-pages\"'))
```

If you are like me who loves using the shell, you can use the following below
to execute the Ruby script above by using `ruby -e`.

```sh
ruby -e "text = File.read('Gemfile')
File.write('Gemfile', text.gsub(/gem\ \"jekyll\"/, '#gem \"jekyll\"'))
text = File.read('Gemfile')
File.write('Gemfile', text.gsub(/# gem\ \"github-pages\"/, 'gem \"github-pages\"'))"
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Choose a GitHub Pages themes

Once the Gemfile has been setup to use GitHub Pages for the local Jekyll
environment, you can apply the next following Ruby script from the shell to
configure the  Jekyll (`_config.yml`) configuration file `theme:` key with the
value of chosen GitHub Pages theme.

### Architect

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-architect'))"
```

### Cayman

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-cayman'))"
```

### Dinky

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-dinky'))"
```

### Hacker

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-hacker'))"
```

### Leap Day

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-leap-day'))"
```

### Merlot

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-merlot'))"
```

### Midnight

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-midnight'))"
```

### Minimal

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-minimal'))"
```

### Modernist

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-modernist'))"
```

### Slate

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-slate'))"
```

### Tactile

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-tactile'))"
```

### Time Machine

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('_config.yml')
File.write('_config.yml', text.gsub(/theme: minima/, 'theme: jekyll-theme-time-machine'))"
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Modify the layout value for site index page

To apply your Jekyll site with this GitHub Pages theme, execute the following
Ruby script in the shell below.

```sh
ruby -e "text = File.read('index.md')
File.write('index.md', text.gsub(/layout: home/, 'layout: default'))"
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Update all your gems

Once you have configured your preferred GitHub Pages theme using Ruby above,
you will need get `bundle` to assist you in updating all the necessary gems for
GitHub Pages.

```sh
bundle update
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Preview your site

Without any particular issue that may arise after you requested `bundle` to
update all necessary gems, you can use the following command to execute Jeykll
locally to serve and preview the site on 
[http://localhost:4000/](http://localhost:4000/]).

```sh
bundle exec jekyll serve
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Conclusion

Once you are satisfied with the chosen GitHub Pages theme tested locally, use
`git` to add all modified files, commit the changes and push to your GitHub
repository.

```sh
git add --all
git commit -m 'Updated Jekyll with GitHub Pages themes'
git push -u origin master
```

After you have pushed, you can view it live from your
`$GITHUB_USERNAME`.github.io URL using your favourite browser. It just that
easy with keyboard.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
