# @Shizen

Supposedly this is part of a "thing" on github for making sites dedicated/related to or describing a project, or in this case a user (me).  The overall system is called `GitHub Pages`.

- [Building User/org Page Sites](https://help.github.com/articles/user-organization-and-project-pages/)
- [What is GitHub Pages](https://help.github.com/articles/what-is-github-pages/)

[See more](Another.md)

It seems to take 1+ minutes for the associated github.io page to update.  The documentation implies there's some complication going on, although thus far I haven't encountered any particularly description thereof.

Blog Tests:  
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
