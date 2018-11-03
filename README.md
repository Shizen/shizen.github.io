# Shizen

[Shinworks](www.shinworks.co "My 'professional' website")   
[Satsuma](www.satsuma.me "My personal website")  
[GitHub](https://github.com/Shizen/shizen.github.io)
[LinkedIn]( "I don't use LinkedIn, really, but people seem to like it ;)")  

Shizen, aka Shin is a full stack developer and free lance computer consultant (as in software developer--not sure when that terminology got conflated) living in Seattle, WA (USA).

Categories:  
{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

---

`GitHub Pages`:  (*inception!*)

(These notes are really for myself, but if they prove useful to you... Enjoy!)

This is the landing page on github for making sites dedicated/related to or describing a project, or in this case a user (me).  The overall system is called `GitHub Pages`.  So this page should be all about me :tm:... As I relate to github.  or something.

`GitHub Pages` uses [`jekyll`](https://github.com/jekyll/jekyll) as its static site constructor which in turn uses [`liquid`](https://shopify.github.io/liquid/basics/introduction/) to do some limited dynamic content.  Since github claims `jekyll` does no server-side anything (seems likely as a design constraint), one presumes Liquid is entirely client-side scripting.  The system is similar in design to `sites` (less many features, plus many users ;).

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

