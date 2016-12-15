# PredixTrainNote

# 介绍
  本文是参加上海Predix培训及动手实验后的相关经验总结及整理。
# Predix动手实验内容
## 实验1：压缩机数据展示
### 目的及内容
  + 目的
      + 熟悉创建数据库的过程
      + 了解如何向数据库导入数据
      + 了解SpringBoot/nodeJs后台项目的开发和部署
      + 了解PredixUI的使用和简单的开发
  + 内容
      + 创建数据库Service
      + 导入数据
      + 创建后台项目
      + 创建前台项目
### 实验步骤
#### 准备数据库服务
  + 创建数据库Service
```
cf cs postgres shared <数据库服务名称>
```
  + 创建数据库Service-Key用以查看数据库信息
```
cf csk <数据库服务名称> <创建的key名称>
```
  + 查看并记录创建好的数据库信息
```
cf service-key <数据库服务名称> <创建的key名称>
```

返回如下信息，请保存host/port/database/username/password等信息，后续步骤需要使用

```
{
 "database": "d9657fe33a36c4eb18f8a3d4066bfd841",
 "host": "10.120.8.137",
 "instance_id": "6e801755-418f-4b29-a137-29cadb830d1e",
 "password": "1007352f218140368934338aba816abd",
 "port": "5432",
 "username": "u9657fe33a36c4eb18f8a3d4066bfd841",
 ...
}
```

  + 部署数据库管理端Web版
  + 解压缩附件中 **数据库管理web.zip** 压缩文件
  + 修改解压缩目录中的 **manifest.yml** 文件

```
applications:
- name: <数据库管理端名称>
  memory: 1G
  instances: 1
services:
 - <数据库服务名称>
```

  + 发布数据库管理端Web版
```
cf push
```
  返回信息如下：
```
...
usage: 1G x 1 instances
urls: cnpc-compress-test.run.aws-jp01-pr.ice.predix.io
last uploaded: Thu Dec 15 04:59:44 UTC 2016
...
```
  urls前加上https为该项目返回地址
 
#### 导入数据
  + 访问上一步骤中部署的数据库管理端Web版
  + 输入数据库的host/port/database/username/password
  + 导入附件中的compress.sql文件
#### 后台项目
  + 解压缩<后台Restful代码.zip>
  + 修改解压缩目录中的 **manifest.yml** 文件

```
applications:
- name: <后台服务名称>
  memory: 1G
  instances: 1
services:
 - <数据库服务名称>
```
  + 打包后台代码
```
mvn package
```
  + 发布后台代码
```
cf push
```
  返回信息如下：
```
...
usage: 1G x 1 instances
urls: cnpc-compress-test.run.aws-jp01-pr.ice.predix.io
last uploaded: Thu Dec 15 04:59:44 UTC 2016
...
```
  urls前加上https为该后台服务的访问地址
  相关可以参考：[该项目在GitHub的主页](https://github.com/PredixDev/predix-rdbr-cf)

#### 前台项目
  + 解压缩<前台页面代码.zip>至目录
  + 修改解压缩目录中的 **manifest.yml** 文件
```
applications:
- name: <前端服务名称>
  memory: 1G
  instances: 1
```
  + 替换解压缩目录中\public\elements\views\compress-view.html文件中<后台服务地址>部分为上一步骤中后台服务地址
  + 在压缩目录中执行
```
npm install
bower install
```
  + 最后执行
```
cf push
```
  上传该包至服务器
  返回信息如下：
```
...
usage: 1G x 1 instances
urls: cnpc-compress-test.run.aws-jp01-pr.ice.predix.io
last uploaded: Thu Dec 15 04:59:44 UTC 2016
...
```
  urls前加上https为该后台服务的访问地址
  相关可以参考：[该项目在GitHub的主页](https://github.com/PredixDev/predix-seed)

## 实验2：简单算法开发及发布部署
### 目的及内容
  + 目的
    + 熟悉创建算法过程
    + 了解算法的使用
  + 内容
    + 创建数据分析目录
    + 创建数据分析运行时
    + 开发算法代码
    + 使用Postman等工具上传，发布算法
### 实验步骤
  + 创建算法目录及运行环境
    + 参见[Analytics Catalog Service Setup](https://predix-io.run.aws-jp01-pr.ice.predix.io/docs/?r=524849#ZJLU5uS)
    + 参照其中流程进行步骤5:Bind your application to the service instance后，需要准备postman或者使用[PredixToolKit](https://predix-starter.run.aws-jp01-pr.ice.predix.io/#!/analyticsCatalog)继续进行后续步骤
    + 步骤9:Add an analytic to the catalog中的Java analytic (demo adder java)可以使用附件中<分析算法测试代码.zip>的项目。
    + 算法实现相关要求及目录结构说明请参见[PredixAnalyticsSample](https://github.com/PredixDev/predix-analytics-sample)
