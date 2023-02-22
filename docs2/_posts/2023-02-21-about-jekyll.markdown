---
layout: post
title:  "About Jekyll!"
date:   2023-02-21 10:41:04 +0800
tags:	Jekyll GithubPages
categories: jekyll update
---




### Jekyll

你可以查看 [Jekyll docs][jekyll-docs] 了解有关如何充分利用Jekyll的更多信息。可以在 [Jekyll’s GitHub repo][jekyll-gh]提交bug/功能请求。如果你有其他问题，你可以试着在这里提问 [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/



## [创建帖子](https://jekyllrb.com/docs/posts/#creating-posts)

该`_posts`文件夹是您的博客文章所在的位置。你通常用[Markdown](https://daringfireball.net/projects/markdown/)写帖子，也支持 HTML。

`_posts`要创建帖子，请使用以下格式将文件添加到您的目录：

```
YEAR-MONTH-DAY-title.MARKUP
```

其中`YEAR`是一个四位数，`MONTH`和`DAY`都是两位数，`MARKUP`是表示文件中使用的格式的文件扩展名。例如，以下是有效帖子文件名的示例：

```
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.md
```

[所有博客文章文件都必须以front matter](https://jekyllrb.com/docs/front-matter/)开头，这通常用于设置[布局](https://jekyllrb.com/docs/layouts/)或其他元数据。对于一个简单的例子，这可以是空的：

```
---
layout: post
title:  "Welcome to Jekyll!"
---

# Welcome

**Hello world**, this is my first Jekyll blog post.

I hope you like it!
```

##### ProTip™：链接到其他帖子

使用[`post_url`](https://jekyllrb.com/docs/liquid/tags/#linking-to-posts) 标签链接到其他帖子，而不必担心网站永久链接样式更改时 URL 会中断。

##### 注意字符集

内容处理器可以修改某些字符，使它们看起来更漂亮。例如，`smart`Redcarpet 中的扩展将标准的 ASCII 引号字符转换为卷曲的 Unicode 字符。为了让浏览器正确显示这些字符，请通过`<meta charset="utf-8">`在 `<head>`您的布局中包含来定义字符集元值。

## [包括图像和资源](https://jekyllrb.com/docs/posts/#including-images-and-resources)

在某些时候，您会希望在文本内容中包含图像、下载或其他数字资产。一种常见的解决方案是在项目目录的根目录中创建一个文件夹，名称类似于`assets`，其中放置任何图像、文件或其他资源。然后，在任何帖子中，它们都可以链接到使用站点的根目录作为要包含的资产的路径。执行此操作的最佳方法取决于您站点的（子）域和路径的配置方式，但以下是 Markdown 中的一些简单示例：

在帖子中包含图像资产：

```
... which is shown in the screenshot below:
![My helpful screenshot](/assets/screenshot.jpg)
```

链接到 PDF 供读者下载：

```
... you can [get the PDF](/assets/mydoc.pdf) directly.
```

## [显示帖子索引](https://jekyllrb.com/docs/posts/#displaying-an-index-of-posts)

[多亏了Liquid](https://shopify.github.io/liquid/)及其标签，在另一个页面上创建帖子索引应该很容易 。这是一个简单的示例，说明如何创建指向您的博客文章的链接列表：

```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
```

您可以完全控制如何（以及在何处）显示您的帖子，以及如何构建您的网站。如果你想了解更多，你应该阅读更多关于[模板如何与 Jekyll 一起工作的内容。](https://jekyllrb.com/docs/templates/)

请注意，该`post`变量仅存在于`for`上面的循环中。`for`如果您希望访问当前呈现的页面/帖子的变量（其中包含循环的帖子/页面的变量），请改用该`page` 变量。

## [标签和类别](https://jekyllrb.com/docs/posts/#tags-and-categories)

*Jekyll 对博客文章中的标签*和*类别*提供了一流的支持。

### [标签](https://jekyllrb.com/docs/posts/#tags)

`tag`帖子的标签在帖子的前面使用单个条目或`tags`多个条目的键定义 。
由于 Jekyll 期望多个项目映射到 key `tags`， 如果它包含空格，它会自动*拆分一个字符串条目。*例如，虽然 front matter `tag: classic hollywood`将被处理成一个单一的实体 `"classic hollywood"`，但 front matter`tags: classic hollywood`将被处理成一个条目数组`["classic", "hollywood"]`。

无论选择的前键如何，Jekyll 都会存储映射到公开给 Liquid 模板的复数键的元数据。

当前站点中注册的所有标签都通过 公开给 Liquid 模板 `site.tags`。在页面上迭代`site.tags`将产生另一个包含两个项目的数组，其中第一个项目是标签的名称，第二个项目是具有 该标签的*帖子数组。*

```
{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
```

### [类别](https://jekyllrb.com/docs/posts/#categories)

类似于上述标签的帖子作品类别：

- `category`它们可以使用键或 `categories`（遵循与标签相同的逻辑）通过前端定义
- 站点中注册的所有类别都暴露给 Liquid 模板，通过 `site.categories`这些模板可以迭代（类似于上面的标签循环）。

*然而，类别和标签之间的相似性到此为止。*

与标签不同，帖子的类别也可以由帖子的文件路径定义。上面的任何目录都`_posts`将作为类别读入。例如，如果帖子位于 path `movies/horror/_posts/2019-05-21-bride-of-chucky.markdown`，则`movies`和`horror`将自动注册为该帖子的类别。

当帖子也有 front matter 定义类别时，如果它们不存在，它们只会被添加到现有列表中。

类别和标签之间的标志性区别在于帖子的类别可以合并到为帖子[生成的 URL](https://jekyllrb.com/docs/permalinks/#global)中，而标签不能。

因此，根据前面内容是否有`category: classic hollywood`, 或`categories: classic hollywood`，上面的示例帖子的 URL 将分别为 either `movies/horror/classic%20hollywood/2019/05/21/bride-of-chucky.html`或 `movies/horror/classic/hollywood/2019/05/21/bride-of-chucky.html`。

## [帖子摘录](https://jekyllrb.com/docs/posts/#post-excerpts)

`excerpt`您可以通过在帖子上使用变量来访问帖子内容的片段。默认情况下，这是帖子中的第一段内容，但是可以通过`excerpt_separator`在 front matter 或 中设置变量 来自定义它`_config.yml`。

```
---
excerpt_separator: <!--more-->
---

Excerpt with multiple paragraphs

Here's another paragraph in the excerpt.
<!--more-->
Out-of-excerpt
```

下面是输出带有摘录的博客文章列表的示例：

```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
```

## [草稿](https://jekyllrb.com/docs/posts/#drafts)

草稿是文件名中没有日期的帖子。它们是您仍在处理但还不想发布的帖子。要启动并运行草稿，请`_drafts`在您站点的根目录中创建一个文件夹并创建您的第一个草稿：

```
.
├── _drafts
│   └── a-draft-post.md
...
```

要使用草稿预览您的站点，请运行`jekyll serve`或`jekyll build` 使用`--drafts`开关。每个人都将被分配草稿文件的值修改时间作为其日期，因此您将看到当前编辑的草稿作为最新帖子。
