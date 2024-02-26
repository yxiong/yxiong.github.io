# Ying Xiong's Blog

This repo holds the content of my personal blog at https://yxiong.github.io/.

## Setup

* The repo is hosted at https://github.com/yxiong/yxiong.github.io.
* The repo enabled [GitHub Pages](https://pages.github.com/), such that any changes to the `main` branch will automatically be published to  and published to https://yxiong.github.io/.
* The site is powered by [Jekyll](https://jekyllrb.com/), and the repo was originally forked from https://github.com/skills/github-pages.
* To add math support, I followed [this post](https://stackoverflow.com/questions/5189560) on StackOverflow with some adaptation:
  * Add a `_includes/head.html` file based on the content of https://github.com/jekyll/minima/blob/master/_includes/head.html.
    * I need to change the `/assets/css/style.css` to `/assets/main.css`, or otherwise the style won't apply. It took me an hour to figure this out, but I still do not understand the root cause.
  * Add a `_includes/custom-head.html` file with the MathJax javascript.
