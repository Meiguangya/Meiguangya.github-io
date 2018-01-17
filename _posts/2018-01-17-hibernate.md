---
layout: post
title: hibernate
tags: hibernate
---

# Hibernate更新数据
---
### Hibernate中的三种状态

在hibernate中，对象的存在三种状态 Transient(瞬时状态)、Persistent（持久化状态）、Detached（脱管/游离状态）。

1. Transient(瞬时状态)：处于瞬时状态时，对象只存在于JVM内存中，并没有和Hibernate中的Session关联，没有纳入到Hibernate的缓存管理中去，在数据库中也没有与对象对应的记录。如：新建一个对象时，该对象就处于瞬时状态。
2. Persistent：处于持久化状态时，对象不仅在内存中占有空间，Hibernate缓存Session中也存在该对象，并且数据库表中有与该对象对应的记录，主键值确定。
3. Detached：处于游离状态时，对象在内存中存在，在数据库中有与之对应的记录，但是不存在于Session缓存中 示例如下：

***
```
//  新建一个User类对象，此时user处于瞬时状态（Transient）
    User user=new User();
    user.setId(1);
    user.setName("Luvjuin");
    //  获取Session
    Configuration cfg=new Configuration().configure();
    SessionFactory sf=cfg.buildSessionFactory();
    Session session=sf.openSession();
    //开启事务
    session.beginTransaction();
    //保存user对象到数据库
    //事务提交后，对象将会处于持久化状态（Persistent）
    session.save(user);
    //事务提交
    session.getTransaction().commit();
    //关闭Session和SessionFactroy
    //此时user对象将处于游离状态（Detached）
    session.close();
    sf.close();
```

4.三种状态之间的转换
![](http://img.blog.csdn.net/20170504142327268?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTHV2anVpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* transient：是由new 命令开辟的内存对象，意义仅是携带信息的载体，不和数据库中的数据有任何的关联。
* persistent：持久的实例在数据库中有对应的记录，拥有一个持久化标识。在执行commit之后，才在数据据库中真正运行SQL进行变更
* detached：与持久对象关联的Session被关闭后，对象变为托管的。对托管对象的引用依然有效，对象可以继续被修改。托管对象如果重新关联到某个新的Session上，会再次转变为持久化对象。托管期间的改动将会被持久化到数据库。

### 常用更新数据方法

* Persistent（持久态）下对象没有变化则不会发出SQL语句，（即使手动使用update方法也不会发出update语句），有变化则自动发出update方法，更新所有字段。不需要手动调用update方法。
* Detached状态下不管JVM内存中对象的属性值和数据库表中的值是否一致，调用update方法后一定会发出一条update语句，这和Persistent状态下是不一致的

* Transient状态下更新数据要指定主键，否则不能更新

- 如果只想要更改几个字段
-
1. xml中设置property标签update="false"
2. dynamic-update="true"
3. @DynamicUpdate注解
`@DynamicUpdate`

4. 使用HQL语句（灵活，方便）

```
String hql = "update Student s set s.age = 18 where id = 3";
session.beginTransaction();
Query query = session.createQuery(hql);
query.executeUpdate();
session.getTransaction.commit();
```
***
思考：如果创建一个新的对象，想用这个对象替换掉数据库中已有的数据，如果这个对象有几个
字段是空的，那会不会直接赋值为空，如果是，那么怎么把这些空的值设置为原来那条数据的值呢？实际应用就是编辑个人信息，不能编辑密码，所以从表单中获取的数据，密码等有些字段是空的，那么只更改一些字段信息....
spring mvc 中的modelAttribute注解的作用可以和这个结合起来。