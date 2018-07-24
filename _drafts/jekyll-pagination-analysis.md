---
layout: post
title: jekyll-paginate分页问题
tags: paginate，jekyll
date: 2018-07-23 17:43:58 +0800
categories: Jekyll
---

config.yml下配置信息：

    gems: [jekyll-paginate]
    paginate: 12
    paginate_path: "page/:num/"

需要分页的页面需要引入

{% include posts_paginator.html %}

posts_paginator.html的内容如下：

    {% for post in paginator.posts %}
    <article class="excerpt excerpt-c5">
        <a class="thumbnail" href={{ post.url }}>
            <img src="/assets/images/cover.jpg" class="thumb" style="display: inline;">
        </a>
        <h2>
            <a href={{ post.url }}>{{ post.title | strip_html | strip_newlines | truncate: 250 }}&hellip;
            </a>
        </h2>
        <footer>
            <a href="javascript:;" class="post-like" data-pid="135" etap="like">
                <i class="fa"></i><span>&chi;</span>
            </a>
            <time class="hot">Hot</time>
            <span class="post-view">{{ post.date | date: "%F"}}</span>
            <span class="post-comm">{{ post.tags }}</span>
        </footer>
    </article>
    {% endfor %}
    {% include pagination.html %}

pagination.html显示的是页码内容：

    <!-- 分页链接 -->
    <div class="pagination">
      {% if paginator.previous_page %}
        <a href="{{ paginator.previous_page_path }}" class="previous" style="float:left;"><< Previous</a>
      {% else %}
        <!-- <span class="previous">Previous</span> -->
      {% endif %}
        <span class="page_number ">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
      {% if paginator.next_page %}
        <a href="{{ paginator.next_page_path }}" class="next" style="float:right;margin-right:30px;">Next >></a>
      {% else %}
        <!-- <span class="next ">Next</span> -->
      {% endif %}
    </div>

自此，项目目录下的分页即可显示。

需要注意的是，分页引入的入口文件只能在index.html/md/markdown 文件中。为何，官方规则。

那么问题出来了，我要是在别的页面显示分页，该如何操作呢?

- 分页只在html文件中起作用
 
- paginate_path同时定义了需要被分页的文件，本人测试这个叫index.html，具体目录由paginate_path中的路径定义，如果定义的目录没有，则会向上寻找index.html，直到根目录的index.html，具体机制官网上没有详细说，所以还需要进一步实验，

也许你会说，先之前那样把分页引入页面不就行了。

太天真了，no，答案是不行的。

具体目录由paginate_path中的路径定义，把paginate_path改成数组行不。no，也是不行的。看后边

不过有意思的是：paginate_path 设置为"page/:num"时，跟站点下的分页显示正常；设置为“archive/page/:num”时，主页显示不正常，而archive下的分页显示正常

期初我也这么搞得，就是出不来哦！

在网上搜索找到了答案，但是，我没看明白，实际操作了下，还有报错什么的。就这个分页搞了两天，我休闲的周六日，就这样拜倒在分页下。感觉比我追女朋友还费神。

不过好在最后实现了。改了下插件的逻辑实现的。一种成功的兴奋油然而生。

pagination.rb文件修改：

      # def generate(site)
      #   if Pager.pagination_enabled?(site)
      #     if template = template_page(site)
      #       paginate(site, template)
      #     else
      #       Jekyll.logger.warn "Pagination:", "Pagination is enabled, but I couldn't find " +
      #       "an index.html page to use as the pagination template. Skipping pagination."
      #     end
      #   end
      # end

      def generate(site)
        if Pager.pagination_enabled?(site)

          path_array = site.config['paginatepath']
          length = path_array.length-1
          for i in 0..length do
            site.config['paginate_path'] = path_array[i]
            if template = template_page(site)
              paginate(site, template)
            else
              Jekyll.logger.warn "Pagination:", "Pagination is enabled, but I couldn't find " +
              "an index.html page to use as the pagination template. Skipping pagination."
            end
          end
        end
      end

原来，paginate_path是个字符串设置不了数组

自此多个页面分页显示就正常了！