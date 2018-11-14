# Shizen

[Shinworks](https://www.shinworks.co "My 'professional'/D.B.A. website")   
[Satsuma](https://www.satsuma.me "My personal website")  
[GitHub](https://github.com/Shizen)  
[LinkedIn](https://www.linkedin.com/in/sean-simmons-3932549b "I don't use LinkedIn, really, but people seem to like it ;)")  

Shizen, aka Shin is a full stack developer and free lance computer consultant (as in software developer--not sure when that terminology got conflated) living in Seattle, WA (USA).

I have not historically been much of a "blogger", per se.  These entries here are more sort of an experiment on my part with the capabilities of github/`Github Pages`, which I find interesting for their similarities to my own `sites` project.

<fieldset class="blogs">
  <legend>Blogs:</legend>  
  <ul>
    {% for post in site.posts %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        {{ post.excerpt }}
      </li>
    {% endfor %}
  </ul>
</fieldset>

<p>&nbsp;</p> <!-- evil spacer trix -->

---

### `GitHub Pages`:  (*inception!*)

(<mark>The following notes are really for myself, but if they prove useful to you... Enjoy!</mark>  You might find my [introduction to Github Pages]({{ site.baseurl }}{% post_url 2018-10-16-GitHub-Pages %}) article more edifying)

This is the landing page on github for making sites dedicated/related to or describing a project, or in this case a user (me).  The overall system is called `GitHub Pages`.  So this page should be all about me :tm:... As I relate to github.  or something.

`GitHub Pages` uses [`jekyll`](https://github.com/jekyll/jekyll) as its "static site constructor" which in turn uses [`liquid`](https://shopify.github.io/liquid/basics/introduction/) to do some limited dynamic content.  Since github claims `jekyll` does no server-side anything (seems likely as a design constraint), one presumes Liquid is entirely client-side scripting.  The system is similar in design to `sites` (less many features, plus many users ;).

- [Building User/org Page Sites](https://help.github.com/articles/user-organization-and-project-pages/)
- [What is GitHub Pages](https://help.github.com/articles/what-is-github-pages/)

It seems to take 1+ minutes for the associated github.io page to update.  The documentation implies there's some compilation going on, although thus far I haven't encountered any particularly description thereof.  I'd assume it's basic caching/depoyment delay.  GFM does not provide any toc afaik, and neither does Jekyll, although it's conceivable that some liquid shim might be able to manage it, along with a custom style sheet or layout.  Also note that while Jekyll allows for [tag filter plugins](https://jekyllrb.com/docs/plugins/), **these will not run on GitHub Pages**.  See the `--safe` option documentation (which is the option GitHub Pages is leveraging to disable plugins).

[Other Projects](Another.md)

(Wouldn't it be nice to have a nice fieldset tag?  Shinmark does... sigh;  Of course, GFM allows embedded HTML)

((of course, the styling of this theme overrides the default fieldset look anyway))

<fieldset>
  <legend>Blog Tests:</legend>  
  <ul>
    {% for post in site.posts %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        {{ post.excerpt }}
      </li>
    {% endfor %}
  </ul>
</fieldset>

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}


Categories:  

<ul class="horizontal-list">
  <li>:</li>
{% for category in site.categories %}
  <li>{{ category[0] }} :</li>
{% endfor %}
</ul>

I don't think I can do any clever webAPI/REST type tricks in github pages as I don't have any control over routes, but I haven't looked.  The alternative would be to have hard coded pages for each category.  I really not feeling like this is worth investing time in :).
