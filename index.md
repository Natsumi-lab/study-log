---
layout: default
title: Home
---

<div class="wrapper-two-columns">

  <!-- 左：カテゴリ一覧（sidebar） -->
  <aside class="sidebar">
    <h2>カテゴリ</h2>
    <ul>
      {% for category in site.categories %}
        <li>
          <a href="{{ '/categories/' | relative_url }}#{{ category[0] | slugify }}">
            {{ category[0] }} ({{ category[1].size }})
          </a>
        </li>
      {% endfor %}
    </ul>
  </aside>

  <!-- 右：記事一覧（content-main） -->
  <main class="content-main">
    <h1>記事一覧</h1>

    <ul>
      {% for post in site.posts %}
        <li>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a><br>
          <small>
            {{ post.date | date: "%Y-%m-%d" }}
            {% if post.categories %}
              （{{ post.categories | join: ", " }}）
            {% endif %}
          </small>
        </li>
      {% endfor %}
    </ul>
  </main>

</div>


