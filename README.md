# CAU ACTivity

[![All test](https://github.com/caufande/act/actions/workflows/test-all.yml/badge.svg)](https://github.com/caufande/act/actions/workflows/test-all.yml)
[![Build](https://github.com/caufande/act/actions/workflows/dobuild.yml/badge.svg)](https://github.com/caufande/act/actions/workflows/dobuild.yml)

<!-- [项目网址](https://caufande.github.io/act/) -->

本项目是为中国农业大学烟台研究院设施农业科学与工程 2024 级学生整理的志愿项目和科创比赛等目录。
未来或许会增加服务的学生群体范围，但是凡事都有个发展的过程，所以现在先就整理这些。

## 项目架构

本项目主要内容是一个有关项目和比赛的数据库，围绕数据库有各种小功能。

```plantuml:arch
@startuml "信息流动图"
title 信  息  流  动  图
package "cnblogs 博  客" {
	database "活动列表评论" as db_acts
	database "用  户  组  博  文" as db_groups
	database "活  动  索  引  博  文" as db_version
}
component "编  程  接  口" {
	component "cnblogs API" as dbapi_cnbapi
	component "MetaWeblog" as dbapi_metaweblog
}
db_acts <--> dbapi_cnbapi
db_groups --> dbapi_cnbapi
db_version --> dbapi_cnbapi
db_groups <-- dbapi_metaweblog
db_version <-- dbapi_metaweblog
component "Weapp" {
	portin "构 建 时 secret" as weapp_secret
	database "缓  存  用  户  组" as weapp_groups
	database "缓  存  活  动  列  表" as weapp_acts
	component "当  日  活  动  显  示" as weapp_tody
	weapp_secret --> weapp_groups
	weapp_secret --> weapp_acts
	weapp_acts --> weapp_tody
	weapp_groups --> weapp_tody
}
dbapi_cnbapi -> weapp_secret
component "GitHub Pages" {
	portin "用 户 secret" as pages_secret
	portin "用 户 token" as pages_token
	component "用  户  组  管  理" as pages_groups
	component "活  动  添  加  修  改" as pages_acts
	actor "管  理  员" as pages_admin
	pages_token <-- pages_groups
	pages_secret <-- pages_acts
	pages_groups <- pages_admin
}
dbapi_metaweblog <-- pages_token
dbapi_cnbapi <-- pages_secret
package "信  息  源" {
	actor "活  动  组  织  者" as fm_org
	collections "活  动  发  布  信  息" as fm_info
}
pages_acts <-- fm_org
pages_acts <-- fm_info
pages_admin <-- fm_org
@enduml
```
