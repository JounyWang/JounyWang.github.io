---
layout: home
---

<div class="index-content music">
    <div class="section">
        <ul class="artical-cate">
            <li ><a href="/"><span>Blog</span></a></li>
            <li class="on" style="text-align:center"><a href="/music"><span>Music</span></a></li>
            <li style="text-align:right"><a href="/travel"><span>Travel</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <ul class="artical-list">
        {% for post in site.categories.music %}
            <li>
                <h2>
                    <a href="{{ post.url }}">{{ post.title }}</a>
                </h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
