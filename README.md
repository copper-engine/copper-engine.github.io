copper-engine-website
=====================

This is the GitHub project for the GitHub Pages based website of the **[copper-engine.org](http://copper-engine.org)** project.

This website makes use of the [Jekyll templating engine](http://jekyllrb.com/) which is integrated into GitHub Pages. See [Using Jekyll with Pages](https://help.github.com/articles/using-jekyll-with-pages#configuring-jekyll) for more details.

Whenever you push something into this project, GitHub automatically runs Jekyll and serves the content on [copper-engine.github.io/copper-engine-website/](http://copper-engine.github.io/copper-engine-website/). Note that it may take up to 10 minutes until the new content will be available.

### Using Jekyll locally

To test these webpages, first install Ruby, Bundler and Jekyll by following [these instructions](https://help.github.com/articles/using-jekyll-with-pages#installing-jekyll).

Then clone this project to your local folder and run

    $ cd <your-cloned-project-dir>
    $ bundle exec jekyll serve --watch --baseurl ''

to serve the webpages locally on `http://localhost:4000/`. _(You might need to add `~/.gem/ruby/2.0.0/bin` to your PATH so that the `bundle` executable can be found.)_
