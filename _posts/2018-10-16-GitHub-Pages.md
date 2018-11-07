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


[Jekyll](https://jekyllrb.com/) is a markdown-based templating engine for constructing "low impact" web sites.  By low impact, here, I mean light on server resources.  For `Github Pages`, Github uses Jekyll to serve whatever it finds in the given repo as if it were a Jekyll project.  Basic configuration is accomplished via a yaml file at the site's root for settings like themes.  Jekyll also defines a semantic directory structure (which also applies to Github Page repos) allowing for overriding various theming details via layout and css files.  Finally Jekyll leverages shopify's [liquid](https://github.com/Shopify/liquid/wiki) to provide some measure of server-side scripting.

In the brief outline below, unless otherwise stated, all the features ascribed to Jekyll are also available in the context of Github Pages.

## Jekyll Markdown

Jekyll uses, afaik, GFM markdown (I'm shocked).  Its markdown includes front-matter (that's an optional section at the head of the file where one can add key-value pairs of information).  This front-matter is used (as far as I've seen) for informing liquid about meta data relating to each given file.

What does this mean?

It means that the system is designed to have its users author their website pages in Markdown, and Jekyll will "compile" those pages into HTML and CSS to be served (on github.io).  Markdown links and images follow standard guidelines.  Jekyll/GitHub Pages will simply serve whatever is in the Repos.  I haven't tested if there are issues with relative urls like there are with README markdown files served on `github.com`.* (Actually I have, and there are some.  See below)

Exactly which brand of front-matter Jekyll uses I'm not sure.  In my brief experiences with the system, the key-value pairs set in the front-matter inform aspects of the page's rendering by Jekyll and indexing by Liquid.  For instance, the article is in theory a blog post.  Its front-matter looks like...

```
---
layout: default
title: "Shin does Jekyll"
categories: [jekyll, github, cms]
tags: [sites]
excerpt_separator: <!--excerpt delimeter-->
---
```

`layout` speaks to which layout template to use.  `title` is the title of this page (more or less).  `categories` are indexed by liquid for server-side scripting reference, as are `tags`.  The `excerpt_separator` is also used by `Liquid` when it inserts an excerpt of a file (it tells `Liquid` where the excerpt ends).

Markdown also "normally" allows for <mark>embedded html</mark>.  This means you can literally just write your own html mixed with your markdown (which effectively makes your markup just a macro shorthand for html at that point).  Some systems limit or filter which tags they will allow (for instance github itself).  I haven't researched or tested what limits github pages or jekyll places, but they certainly allow <i>some</i> html, but not <i>all</i> (`<i>all</i>`) ;).

Jekyll also supports tag filters, which I believe is how `Liquid` is included in their ecosystem in the first place.  It is not the only one.  For instance, there is also a filter for syntax coloring--[`rouge`](http://rouge.jneen.net/).  Of course, markdown does this already...

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
Note that while the ruby syntax css appears to be imported by default not all are, and might require [extra effort](https://jekyllrb.com/docs/liquid/tags/#stylesheets-for-syntax-highlighting) ;).  

Apparently there are more complicated combinatorics available via the yaml settings in `_config.yml` (the root level config file mentioned earlier).  In theory, for example, you can use kramdown for instance and specify a dialect.  I haven't looked into the extent of these options.

## [Liquid](https://github.com/Shopify/liquid)

[`Liquid`](https://help.shopify.com/en/themes/liquid/basics) is an open-source server side scripting language written by shopify to serve as a template engine (presumably for their user base).  (Also, Liquid's [github](https://shopify.github.io/liquid/basics/introduction/) link).

Liquid provides some basicaly scripting/flow control.  Object access and angular style filters.  So things like `\{\{ 50 | money_with_currency }}` : {{ 50 | money_with_currency }}.  The primary limiter on what one can do depends on what objects GitHub Pages/Jekyll provides to Liquid to manipulate.  A concise listing thereof I haven't seen, yet ([best so far](https://jekyllrb.com/docs/variables/)).

Despite reading somewhere that Jekyll is supposed to be 100% static, does allow for dynamic content/server-side scripting.  Take this example from the [Posts](https://jekyllrb.com/docs/posts/) page in Jekyll.  When I first read this, I assumed they were doing some tricks with transmitting raw information to the client and letting the client process it via client-side scripts, similar to how angular works.  This turns out not to be true, but rather, the original statement is "false" (or at least misleading).  *Liquid* does server-side processing.  it is in fact a server-side scripting language (they call themselves a template engine).  Take the following example.

```html
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
```

Which then gets rendered (in the context of the `shizen.github.io` repo) the following

---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

---

This format also feels very angular like to me.  Ideally, we'd get some sort of nav infrastructure as well.  I'm sure this is just a matter of ignorance on my part, but I haven't seen anything of the sort yet.

## Gotchas

So with the basics understood we can deal with a few gotchas I've noticed.

### Links

Links and relative urls are often problematic in these systems as they move things around during their internal deployment step and do not always map the urls correctly.  For instance...

[Back to Head](/README.md) (`[Back to Head](/README.md)`): Notice how this link doesn't serve up the parsed markdown page (documentation I've read claims that markdown files are compiled as part of the "compilation and posting" phase that happens every `post-commit`.  This is technically an absolute url (the leading `/`).  `[Back to Index](README.md)` (which is relative) actually does resolve to the *compiled* (I prefer the term rendered) markdown page.  This despite that in theory, in my repo structure these two urls are equivalent.

For their part, Jekyll offers the following work around, using Liquid.  `[Back to Head]({{ site.baseurl }}{% link README.md %})` which produces the link [Back to Head]({{ site.baseurl }}{% link README.md %}).

Also,in theory the shorthand `\{% link README.md %}` should work as well. E.g. `[README](\{% link README.md %})` [README]({% link README.md %}).

### Excerpts

In addition to its page_url processing, liquid & jekyll provide "tag filters".  I suspect the excerpt above was one example.  Unfortunately, the default template does not deal with excerpts at all, and thus you end up with an H1 quoted in your documents, because it takes the markdown literally.  Some clever css could potentially clean this up (by overriding display traits)

### Escaping

As server side expansions, Liquid instructions (they don't seem to have any particularly terminology to talk about their "scripting language") are expanded effectively out of context (before, I think, but not tested) from the markdown itself.  This means that even script in back ticks `` ` `` will be resolved and their resolved values rendered to the page.

## Overriding and Semantic Directory Structure in Jekyll

Apparently if I create a `_drafts/` directory, whatever I put in there won't be served, and I can later move it to the posts directory and rename it.  (And here I've skipped over the part where blog posts are identified by their inclusion in the `_posts` directory.  If I had jekyll setup to run locally I could override this behavior in its cli.  This obviously doesn't apply to Github Pages.  

Another example would be the `layout` front-matter described above.  This is a direct reference to an html file in the `_layouts` directory that Jekyll is going to reference to build the page (very similar to `renderer` in `sites`).  The `slates` theme (which is what this site uses and is blended with our repo during rendering/deploy) doesn't define any layout other than default.  Thus it gets rendered without any templating.

To introduce a navigation to my template, I could build a new layout, build my own theme, or use the override mechanism to override the default layout by providing my own in a local `/_layouts/` directory.  I could add a `post.html` template to support the `layout:post` front-matter suggested in the GitHub-pages docs.  There are [more details](https://jekyllrb.com/docs/themes/) on themes and stuff one can do with them.  Beware that GitHub Pages do not support all themes, although you can reference other github hosted themes with `remote_theme`...  I haven't played with it to test its reach.

You can also add `css` via the `assets/css` directory (which I did for an experiment on another page in this repo).  This example is describe [here](https://help.github.com/articles/customizing-css-and-html-in-your-jekyll-theme/).

Github provides a number of internal [`Github Pages` help files](https://help.github.com/categories/customizing-github-pages/).
