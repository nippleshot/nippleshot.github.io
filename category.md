---
layout: article
titles:
  # @start locale config
  en      : &EN       Category
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  zh-Hans : &ZH_HANS  类目
  zh      : *ZH_HANS
  zh-CN   : *ZH_HANS
  zh-SG   : *ZH_HANS
  zh-Hant : &ZH_HANT  類目
  zh-TW   : *ZH_HANT
  zh-HK   : *ZH_HANT
  ko      : &KO       카테고리
  ko-KR   : *KO
  fr      : &FR       Category
  fr-BE   : *FR
  fr-CA   : *FR
  fr-CH   : *FR
  fr-FR   : *FR
  fr-LU   : *FR
  # @end locale config
key: page-category
---

<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>

    <h3 class="category-head">{{ category_name }}</h3>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      <p style="line-height:1rem">
        <a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a>
        <span style="font-size: 1rem; font-weight:normal">{{post.date | date: "%b %d %Y"}}</span>
      </p>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>
