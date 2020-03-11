---
title: "端口监控"
linkTitle: "端口监控"
date: 2020-02-23
description: >
  端口监控在夜莺中通过配置具体的采集策略来指定采集哪些端口，也支持读取目标机器标识文件
---


支持两种配置方式，一个是在目标机器的指定目录创建元信息文件，一个是在页面上配置采集策略。

## 目标机器元信息文件方式

可以查看collector的配置文件，里边有个portPath配置项，可以把要监听的端口touch一个文件放到portPath指定的路径下，collector组件会自动探测，自动采集。

比如要采集n9e这个服务的monapi组件的端口5800的存活性，可以到portPath指定的路径（没有的话手工创建出对应目录）下，创建一个10_5800的文件，里边的内容是n9e-monapi。

- 文件名命名规范`${采集频率}_${采集的端口}`
- 文件内容是这个模块的名称，比如n9e-monapi，这样报警的时候我们看到n9e-monapi就知道是哪个服务出问题了，不允许有空格，引号等特殊字符

对于业务服务来说，我们在部署服务的时候，可以将这个端口文件创建出来，这样平台就可以帮我们监控这个服务了，当我们下线服务的时候，把这个端口文件顺便删除，这样平台就不再监控这个端口了。

端口采集完了，我们可以在所负责的服务的公共父节点配置一条监控策略，就可以搞定我们手下的所有的服务的所有的模块的所有的端口的监控：

> 采集周期假设为20秒，策略统计周期可以配置为60秒，每个值都=0则报警，端口监控的metric是:proc.port.listen无需每个端口分别配置。

## 页面上配置采集策略

这个方式比较简单，不过多赘述，采集策略配置的时候有个service字段，需要填写模块的名称，相当于目标机器元信息文件的内容部分。
