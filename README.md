# 使用说明

## Jekyll 是什么?

jekyll 是一个静态网站生成器. jekyll 通过 标记语言 markdown 或 textile 和 模板引擎 liquid 转换生成网页. github 为我们提供了这个一个地方, 可以使用 jekyll 做一个我们自己的网站.

jekyll 依赖于 ruby

## 文件介绍

- \_config.yml: jekyll 的全局配置在 \_config.yml 文件中配置. 比如网站的名字, 网站的域名, 网站的链接格式等等。
- \_includes: 对于网站的头部, 底部, 侧栏等公共部分, 为了维护方便, 我们可能想提取出来单独编写, 然后使用的时候包含进去即可.这时我们可以把那些公共部分放在这个目录下.使用时只需要引入即可.

```
{ % include filename % }
```

- \_layouts: 这里面存放的是布局文件，在发布新内容是，指定使用的模板即可。

```
layout: post
```

指定需用的模板。

- index.html: 就是你的默认主页，第一次访问的时候，默认打开的页面。
  > 全局变量

## 本地运行

```
bundle exec jekyll s
```

## 说明文档

- https://chirpy.cotes.page/posts/write-a-new-post/

# 图片剪切

- https://realfavicongenerator.net/
