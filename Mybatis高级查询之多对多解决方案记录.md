---
title: mybatis高级查询之多对多解决方案记录
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
最简单的多对多的配置的方法就是参照一对多的方法，直接写复杂的sql进行，然后**映射**到具体的实体类，大多数都是这种解决方案。但是还有一对多的第二种方法是运用嵌套查询（主查询和从查询即含懒加载的方法）。多对多同样存在这种嵌套查询的方法。
解决方法就是：因为我们多对多存在中间表，所以我们可以多端写SQL的时候进行连表查询，然后直接映射到多端的实体类。下面是例子。
文章类和标签类以及分类是多对多的情况，即存在中间表，中间表是文章-分类表和文章-标签表。现在我们要查询一篇文章下它属于什么分类含有哪些标签。就可以这样配置
- 文章xml
```xml
	<resultMap id="ArticleCateList" type="xyz.quinntian.aurora.model.Article" extends="BaseResultMap">
    <collection property="categories" fetchType="lazy" column="{artId = article_id}"
    select="xyz.quinntian.aurora.mapper.CategoryMapper.selectCateListByArtId" />
    <collection property="tags" fetchType="lazy" column="{artId = article_id}"
    select="xyz.quinntian.aurora.mapper.TagMapper.selectTagListByArtId"/>
  </resultMap>
  <!--嵌套查询分类和标签-->
  <select id="selectArtAndCateByArtId" resultMap="ArticleCateList">
    select
      a.article_id,
      a.article_content,
      a.article_title,
      a.blog_id,
      a.post_date,
      a.sava_date,
      a.`status`,
      a.article_content_md,
      a.article_url
      from aurora_article a
      where a.article_id = #{articleId,jdbcType=BIGINT}
  </select>
```
- 分类xml
```xml
 <!--连表查询 根据博客ID查询所有分类 由于是多对多所以要链表-->
  <select id="selectCateListByArtId" resultMap="BaseResultMap">
    select
    c.category_id,
    c.category_name,
    c.category_url,
    c.blog_id
    from aurora_category c
    inner join aurora_article_category ac
    on c.category_id = ac.category_id
    where ac.article_id = #{artId,jdbcType=BIGINT}
  </select>
```
- 标签xml
```xml
 <select id="selectTagListByArtId" resultMap="BaseResultMap">
   select
    t.tag_id,
    t.tag_name,
    t.tag_url
    from aurora_tag t
    inner join aurora_article_tag ats
    on t.tag_id = ats.tag_id
   where ats.article_id = #{artId,jdbcType=BIGINT}
  </select>
```
主要是，在标签xml和分类的xml我们进行了连表，否则因为是多对多，两张表的关系在中间表记录，即如在分类表中并没有文章ID，如果不进行连表的话无法查询。