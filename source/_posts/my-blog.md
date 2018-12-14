---
title: sharding-jdbc集成springboot
date: 2018-11-16 09:22:40
categories: mysql
tags: [java,springboot,mysql]
---
springboot2.X中使用的spring5，而spring5中已经弃用RelaxedPropertyResolver相关类，而spring-boot-start-sharding相关
的包是依赖于RelaxedPropertyResolver的，所以会导致无法启动，只能用sharding-jdbc-core这个包，自己在yml文件里写相关
的配置
