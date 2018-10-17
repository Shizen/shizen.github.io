---
layout: post
title: "Shin does Jekyll"
categories: [jekyll, github, cms]
tags: [sites]
---

# GitHub Pages & Jekyll

So, Jekyll supposedly has built-in support for blog posts, and GitHub pages supposedly use Jekyll, so...

In theory this will convert itself into a blog post.  Let's find out.

Jekyll, like `Sites` uses markdown and front-matter.  What they set there is different, however.  `Sites` uses a db/sites file to discover what endpoints are supposed to be open.  Jekyll just serves whatever it finds.  Some of the settings in the sites descriptor, however are in the Jekyll front-matter.  E.g. Renderer & Title for instance.

Markdown links and images follow standard guidelines.  Jekyll/GitHub Pages will simply serve whatever is in the Repos.

Despite the fact that Jekyll is supposed to be 100% static, some Jekyll plugins appear to allow dynamic content/server-side scripting.  Take this example from the [Posts](https://jekyllrb.com/docs/posts/) page in Jekyll.  Without looking I have to assume if those two statements are simultaneously true, there must be some transmission of the data from the server to the client followed by client-side evaluation of the scripts.  (I.e. `site.posts` gets sent to the client which then performs the resolution.  It could be done dynamically on demand, and resolved in place in a fashion analogous to Angular.  That's what I'd do with those requirements).

    <ul>
      {% for post in site.posts %}
        <li>
          <a href="{{ post.url }}">{{ post.title }}</a>
        </li>
      {% endfor %}
    </ul>

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

This format also feels very angular like, of course.

This entire page appears to not be templated.  Ideally, this would not be the case and we'd have some sort of nav infrastrcutre.  I'm sure this is just a matter of ignorance on my part.

[Back to Head](/README.md) : Notice how this link doesn't serve up the parsed markdown page (documentation I've read claims that markdown files are compiled as part of the "compilation and posting" phase that happens every `post-commit`.  Note that in `Another.md` I have a nearly identical link, but it *does* resolve to the compiled page.  Interesting :).

The "workaround" seems to be using Liquid again.  [Back to Head]({{ site.baseurl }}{% link README.md %}) should work... on line 42!

In theory `{% link README.md %}` should work as well. E.g. [README]({% link README.md %}).
