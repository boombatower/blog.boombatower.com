---
created: 1248157889
layout: post
redirect_from:
- article/62/
- node/62/
- custom-drupal-org-css/
title: Firefox userContent.css for drupal.org issue queue
---
After seeing a reference to http://userstyles.org/styles/11133 by tha_sun in IRC I went ahead and played with it a bit. I ended up with a very simple version that that just removes the blocks and makes the issue table full width. While I was at it I customized Google.

If you do not know where to put this checkout, <a href="http://coreygilmore.com/blog/2008/10/23/per-site-custom-css-in-firefox/">per-site custom CSS in Firefox</a>.
```css
@namespace url('http://www.w3.org/1999/xhtml');

@-moz-document url-prefix('http://drupal.org/project/issues/') {
  #spacer {}

  #contentwrapper {
    background: none !important;
  }

  #main {
    margin-left: 5px;
    margin-right: 5px;
  }

  #threecol, #content, #squeeze {
    width: 100% !important;
    margin: 0 !important;
    padding: 0 !important;
  }

  .sidebar .block {
    display: none !important;
  }
}

@-moz-document url('http://www.google.com/') {
  #spacer {}

  #ghead, td[align='left'], td[width='25%'], #body > center > font, input[name='btnG'], input[name='btnI'], #footer {
    display: none;
  }

  #main > center {
    margin-top: 250px;
  }
}
```
