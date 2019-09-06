# easy-jekyll-starter

[![Build Status](https://travis-ci.com/malsf21/easy-jekyll-starter.svg?branch=master)](https://travis-ci.com/malsf21/easy-jekyll-starter)

Hey there! This is a simple template repository for [Jekyll](https://jekyllrb.com). It does two simple things over `jekyll new ...`:

* by default, ejects [minima](https://github.com/jekyll/minima) from its gem wrapper; this makes it easier to see what Jekyll is doing under the hood, and makes it easy to build your own theme
* adds template code for [Travis CI](https://travis-ci.com), which uses [rake](https://github.com/ruby/rake) and optionally [html-proofer](https://github.com/gjtorikian/html-proofer) to check if the site built correctly, and auto-deploys the site to GitHub Pages

Other than that, I basically leave minima untouched.

Why did I make this simple template? I love using Jekyll, but I'm not a huge fan of how minima is now bundled by default into `jekyll new ...`. Firstly, it obscures some of the logic of what Jekyll is doing (via the minima gem), which makes it harder to introduce Jekyll to my friends. Since this template ejects minima out of its gem, it should be easier to understand how `_includes`, `_layouts`, and YAML front matter work!

In addition, it becomes a bit of a pain if you want to override some, but not all, of minima's defaults. Putting the SASS in `_sass` makes it easier to accomplish these kinds of changes.

Finally, I have a few preferences on how I usually run my Jekyll setups: I like using html-proofer to catch egregiously bad HTML (and check for dead links, though it's a bit inconsistent on that); I like aliasing the longer `bundle exec ...` commands to shorter keywords (like `rake build`); and, I Like using Travis CI to let me know if I've committed a bad build of the site. They make my life easier, but are understandably strange opinionated choices.

Currently, this template uses Jekyll v4.0.0.

## Development Setup

Admittedly, this is a little more annoying to use than `jekyll new ...`, but only slightly so. 

To use this template, you'll need to have some sort of Ruby on your system; I recommend that you use [rvm](https://rvm.io/), especially if you work on multiple different Ruby projects. Currently, the Travis build uses Ruby v2.6.3, but anything above v2.4.0 should be fine.

If you haven't already, we're first going to install [bundler](https://bundler.io/):

```sh
$ gem install bundler
```

Then, we'll install all of our dependencies from our `Gemfile`. I haven't committed a `Gemfile.lock` on purpose, so bundle will generate one for you.

```sh
$ bundle
```

Now, you're ready to use the site! You can use `bundle exec jekyll serve` to serve the site, or `bundle exec jekyll build` to build it.

Those commands are a bit of a mouthful, so I've made quick rake shortcuts that will save you a few valuable keystrokes:

```sh
$ rake
# same as running bundle exec jekyll build
$ rake build
# also same as running bundle exec jekyll build
$ rake serve
# same as running bundle exec jekyll serve
```

In addition, I've also added html-proofer, which can test the validity of your HTML. You can use it like so:

```sh
$ rake build
...
$ rake test
Running ["ScriptCheck", "ImageCheck", "LinkCheck"] on ["./_site"] on *.html...
```

## Integrating With Travis

I've packaged a `.travis.yml` that builds the site on a Ruby of your choice. By default, it just runs `bundle exec jekyll build`, and therefore will error if you have a Jekyll build error.

In addition, there's also a commented-out line that runs html-proofer on the built site. This works with varying success - sometimes, HTTP timeouts will signify dead links, and sometimes, they signify bad internet - but it's a useful tool that you can include instead!

There's also a commented-out `deploy` key that automatically builds and deploys the result of `bundle exec jekyll build` to GitHub Pages. This is useful if you're using gems (or other pre-deploy processing) that GitHub Pages' gem doesn't support by default.

```yaml
deploy:
  provider: pages
  skip_cleanup: true
  github_token: $github_token
  local_dir: _site
  on:
    branch: master
```

In order to use it, you'll need to set the `$github_token` to a valid GitHub personal access token as a Travis variable. You can generate a token from [here](https://github.com/settings/tokens).
