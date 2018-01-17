---
layout: post
title: hibernate
tags: hibernate
---

# Hibernate更新数据
---
### Hibernate中的三种状态

> 在hibernate中，对象的存在三种状态 Transient(瞬时状态)、Persistent（持久化状态）、Detached（脱管/游离状态）。

1. Transient(瞬时状态)：处于瞬时状态时，对象只存在于JVM内存中，并没有和Hibernate中的Session关联，没有纳入到Hibernate的缓存管理中去，在数据库中也没有与对象对应的记录。如：新建一个对象时，该对象就处于瞬时状态。
2. Persistent：处于持久化状态时，对象不仅在内存中占有空间，Hibernate缓存Session中也存在该对象，并且数据库表中有与该对象对应的记录，主键值确定。
3. .Detached：处于游离状态时，对象在内存中存在，在数据库中有与之对应的记录，但是不存在于Session缓存中 示例如下：

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