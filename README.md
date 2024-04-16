# Ying Xiong's Blog

This repo holds the content of my personal blog at https://yxiong.github.io/.

## Setup

* The repo is hosted at https://github.com/yxiong/yxiong.github.io.
* The repo enabled [GitHub Pages](https://pages.github.com/), such that any changes to the `main` branch will automatically be published to  and published to https://yxiong.github.io/.
* The site is powered by [Jekyll](https://jekyllrb.com/), and the repo was originally forked from https://github.com/skills/github-pages.
  * The repo uses `minima` layout template. Its source code can be found in https://github.com/jekyll/minima.

### Template

We forked a few files from the [minima](https://github.com/jekyll/minima) template and made some adjustments:

* `_layouts/`:
  * `base.html`: no change
  * `post.html`: add a tag section (see below)
* `_includes/`:
  * `head.html`: I need to change the `/assets/css/style.css` to `/assets/main.css`, or otherwise the style won't apply. It took me an hour to figure this out, but I still do not understand the root cause.
  * `header.html`: removed `trigger` section to avoid showoing too many tag pages in the banner.

### Math Support

To add math support, I followed [this post](https://stackoverflow.com/questions/5189560) on StackOverflow with some adaptation:
* Add a `_includes/custom-head.html` file with the MathJax javascript.

### Tags

I followed [Long Qian's post](https://longqian.me/2017/02/09/github-jekyll-tag/) with the following steps:

* Display tags in post:
  * Add tags in each page's front matter.
  * Add snippet in `_layout/post.html` to show tags under date.
* Create tags index page:
  * Create `site_dir/tag/xxx.md` pages.
  * Add a `_layouts/tagpage.html` page.
  * Remove the trigger section in `_includes/header.html`.

## Notes

* Jekyll tutorial: https://cloudcannon.com/tutorials/jekyll-tutorial/getting-started/
* Useful posts on tags:
  * https://peterroelants.github.io/posts/adding-tags-to-github-pages/
  * https://longqian.me/2017/02/09/github-jekyll-tag/
