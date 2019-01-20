---
title: "Projects"
layout: archive
permalink: /projects/
toc:
  - title: Classifiers
    subfolderitems:
      - page: KernelSVM
        url: /thing1.html
      - page: Gaussian
        url: /thing2.html
      - page: Random Forest
        url: /thing3.html
      - page: K-Means Clustering
        url: /thing3.html
      - page: Natural Language Processing
        url: /thing3.html       
  - title: Neural Networks
    subfolderitems:
      - page: Artificial
        url: /piece1.html
      - page: Convolutional
        url: /piece2.html
      - page: Recurrent
        url: /piece3.html
      - page: Self-Organizing Maps
        url: /piece3.html
      - page: Restricted Boltmann Machine
        url: /piece3.html
      - page: Auto Encoder
        url: /piece3.html

---
#H1[About](/about/)
[KernelSVM](/_posts/KernelSVM)

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
