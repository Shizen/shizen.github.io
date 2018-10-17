---
layout: default
title: "Shin does Jekyll"
categories: [jekyll, github, cms]
tags: [sites]
excerpt_separator: <!--excerpt delimeter-->
---

# GitHub Pages & Jekyll

So, Jekyll supposedly has built-in support for blog posts, and GitHub pages supposedly use Jekyll, so...

In theory this will convert itself into a blog post.  Let's find out.

<!--excerpt delimeter-->

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

Apparently if I create a `_drafts/` directory, whatever I put in there won't be served, and I can later move it to the posts directory and rename it.  if I had jekyll setup to run locally I could override this behavior in its cli.  Meh.  Jekyll does not do server-side anything and thus provides no built-in editor like `sites` does.  This is another much ado about nothing, I think--jekyll just doesn't serve this directory as a special hardcoded cut out.  Moving on.

In addition to its page_url processing, liquid & jekyll provide "tag filters".  I suspect the excerpt above was one example.  Unfortunately, the default template does not deal with excerpts at all, and thus you end up with an H1 quoted in your documents, because it takes the markdown literally.  That aside...

There is a filter for syntax coloring--[`rouge`](http://rouge.jneen.net/).  Of course, markdown does this already...

Rouge/Jekyll filter:  
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

Markdown:  
```ruby
def foo
  puts 'foo'
end
```

(Note that with the lack of templating...  I should go look into that.) < Now fixed.  Note that while the ruby syntax css appears to be imported by default not all are, and might require [extra effort](https://jekyllrb.com/docs/liquid/tags/#stylesheets-for-syntax-highlighting) ;).

So having skimmed the relevant documentation, it looks like the `layout` front-matter is a direct reference to an html file in the `_layouts` directory that Jekyll is going to reference to build the page (very similar to `renderer` in `sites`).  Theslates template doesn't define any layout other than default.  Thus it gets rendered without any templating.  Let's test.

Yes, that did the trick.  So for a custom template, I'd either build my own, or there is a mechanism to override the template by providing my own in a local `_layouts/` directory (root-level presumably).  I can customize the default or add new ontes, in theory.  Like a `post.html` template to support the `layout:post` front-matter suggested in the GitHub-pages docs.

