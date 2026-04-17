---

layout: archive

title: "Blog Posts"

permalink: /posts/

author_profile: true

entries_layout: grid

---

<div class="entries-list">

  {% for post in site.posts %}

    {% include archive-single.html type="grid" %}

  {% endfor %}

</div>