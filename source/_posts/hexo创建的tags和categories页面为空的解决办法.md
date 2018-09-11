---
title: hexo创建的tags和categories页面为空的解决办法
toc: false
date: 2018-04-16 02:26:10
categories:
- methods
tags:
- hexo
---



主题：landscape

添加type以及menu后仍然显示空白的解决办法：

打开landscape/_partial/article.ejs，

在`<div class="article-entry" itemprop="articleBody">`的div内添加：

```ejs
<% if (page.type === "tags") { %>
  <div class="tag-cloud">
    <div class="tag-cloud-title">
    <%- "TOTAl : " + site.tags.length %>
    </div>

    <div class="tag-cloud-tags">
    <%- tagcloud({
      min_font: 12,
      max_font: 30,
      amount: 200,
      color: true,
      start_color: '#555',
      end_color: '#111'
      }) %>
    </div>
  </div>

  <% } else if (page.type === 'categories') { %>

  <div class="category-all-page">
    <div class="category-all-title">
    <%- "TOTAL : " + site.categories.length %>
    </div>

    <div class="category-all">
    <%- list_categories() %>
    </div>

  </div>
<% } %>
```

重新打开即可看到正常显示的标签和分类页。



修改categories页面样式（也可以自己设计修改）：

打开landscape/source/css/_partial/article.styl，在尾部添加：

```css
.category-all-page {
  a:link {
    font-size: 14px;
    color: #333;
    text-decoration: none;
  }
  a:hover {
    font-size: 14px;
    color: #d8d;
    text-decoration: none;
    font-weight: bold;
  }
  .category-all-title { text-align: left; }

  .category-all { 
    margin-top: 20px; 
  }

  .category-list {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .category-list-item { 
    text-align: center;
    display: inline-block;
    margin: 8px; 
    padding: 8px;
    width: 150px;
    position: relative;
    background-color: rgba(237, 237, 237, 0.53);
    border-radius: 1px;
    box-shadow:0px 0px  0px 1px #ccc;
  }

  .category-list-link {
  	color: #333;
  }

  .category-list-count {
    color: #333;
    &:before {
      display: inline;
      content: " ("
    }
    &:after {
      display: inline;
      content: ") "
    }
  }

  .category-list-child { padding-left: 10px; color: #333;}
}


```

