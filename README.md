# 博客微服务数据库设计

## 项目概述

本项目主要用于技术练习与微服务架构探索，当前重点为数据库设计与微服务架构搭建，功能尚未完全实现。

## 技术栈

### 后端技术
- **微服务框架**: SpringCloud Alibaba (Nacos, Gateway, Sentinel, Seata)
- **核心框架**: SpringBoot 3, Graalvm17 (JDK17)
- **数据层**: Mybatis-Plus, HikariCP, Ehcache
- **安全认证**: Shiro, OAuth2, Redisson
- **消息队列**: RabbitMQ
- **工具组件**: Hutool, Drools, Knife4j, Minio, XXL-Job, Fastjson, Apache POI
- **其他**: Spring AOP, Spring AOT, Filebeat

### 前端技术
- **网站端**: Vue3 + Element-plus
- **移动端**: Uniapp
- **PC端**: 规划中

### 数据库
- MySQL
- Redis  
- MongoDB

## 数据库设计

### MySQL 数据库

#### 系统库 (hip_system) - 用户管理

**用户信息表 (admin_info)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | bigint | 主键ID | 自增, 非空 |
| admin_id | bigint | 管理员ID | |
| username | varchar(100) | 用户名称 | 非空 |
| password | varchar(100) | 用户密码 | 非空 |
| user_id | bigint | 用户ID | |
| create_time | datetime | 创建时间 | |
| update_time | datetime | 修改时间 | |
| create_user | bigint | 创建人ID | |

**用户权限表 (sys_permission)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | bigint | 主键ID | 自增, 非空 |
| role_id | int | 角色ID | |
| benefits | varchar(100) | 角色权限 | 非空 |
| permission_remark | varchar(100) | 权限描述 | |

**用户角色表 (sys_role)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | bigint | 主键ID | 自增, 非空 |
| role_id | int | 角色ID | |
| role_name | varchar(20) | 角色名称 | 非空 |
| required_points | int | 所需积分 | |
| create_time | datetime | 创建时间 | |
| update_time | datetime | 修改时间 | |
| is_deleted | tinyint | 删除标志 | 0:未删除 1:删除 |

**用户表 (sys_user)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | bigint | 主键ID | 自增, 非空 |
| user_id | varchar(50) | 用户账号 | |
| openid | varchar(50) | 微信标识 | 非空 |
| username | varchar(50) | 用户名称 | |
| password | varchar(100) | 用户密码 | |
| email | varchar(100) | 电子邮箱 | |
| phone | varchar(15) | 手机号码 | |
| create_time | datetime | 创建时间 | |
| update_time | datetime | 修改时间 | |
| is_deleted | tinyint | 删除标志 | 0:未删除 1:删除 |

**用户角色关联表 (sys_user_role)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | bigint | 主键ID | 自增, 非空 |
| role_id | int | 角色ID | |
| user_id | int | 用户ID | |
| role_category | int | 角色类别 | |
| create_time | datetime | 创建时间 | |
| update_time | datetime | 修改时间 | |

**用户登录日志表 (user_login_log)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 主键ID | 自增, 非空 |
| user_id | int | 用户ID | |
| status | tinyint | 登录状态 | 0:失败 1:成功 |
| msg | varchar(255) | 提示信息 | |
| message | varchar(500) | 描述信息 | |
| login_time | datetime | 登录时间 | |
| ipaddr | varchar(128) | IP地址 | |

#### 支付库 (hip_payment)

**积分支付表 (integral)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 支付ID | 自增, 非空 |
| user_id | bigint | 用户ID | |
| order_id | int | 订单ID | |
| amount | decimal | 支付金额 | |
| payment_method | varchar(20) | 支付方式 | |
| status | tinyint | 支付状态 | 0:成功 1:失败 |
| message | text | 额外内容 | |
| create_time | datetime | 创建时间 | |

**用户积分表 (user_integral)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 积分ID | 自增, 非空 |
| user_id | bigint | 用户ID | |
| integral_change | decimal | 积分变动 | |
| current_integral | decimal | 当前积分 | |
| create_time | datetime | 创建时间 | |

#### 博客库 (hip_blogs)

**浏览历史表 (not_history)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 历史记录ID | |
| user_id | bigint | 用户ID | |
| articles_id | bigint | 博客ID | |
| watch_time | datetime | 观看时间 | |

**标签表 (tags)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 标签ID | |
| tags_name | varchar(20) | 标签名称 | |
| introduce | varchar(255) | 标签介绍 | |
| deleted_at | tinyint | 删除标志 | 0:未删除 1:删除 |

**博客标签关联表 (blog_tags)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 关联ID | |
| blog_id | int | 博客ID | |
| tabs | int | 标签ID | |
| deleted_at | tinyint | 删除标志 | 0:未删除 1:删除 |

**用户关注表 (attention_user)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 关注ID | |
| subscriber_id | bigint | 关注者ID | |
| no_subscriber_id | bigint | 被关注者ID | |
| subscriber_time | datetime | 关注时间 | |

**关注日志表 (attention_user_log)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 日志ID | |
| subscriber_id | bigint | 关注者ID | |
| no_subscriber_id | bigint | 被关注者ID | |
| subscriber_time | datetime | 关注时间 | |
| status | tinyint | 操作状态 | 0:删除 1:关注 |

**用户黑名单表 (blacklist_user)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 黑名单ID | |
| blocker_id | bigint | 屏蔽者ID | |
| no_blocked_id | bigint | 被屏蔽者ID | |
| blocked_time | datetime | 屏蔽时间 | |

**文章收藏表 (attention_collect)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 收藏ID | |
| collect_id | bigint | 收藏者ID | |
| attention_id | bigint | 文章ID | |
| collect_time | datetime | 收藏时间 | |
| image_url | varchar(255) | 封面图片 | |

**文章黑名单表 (blacklist_collect)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | int | 黑名单ID | |
| blocker_id | bigint | 屏蔽者ID | |
| no_blocked | bigint | 被屏蔽文章ID | |
| blocked_time | datetime | 屏蔽时间 | |

### Lucene 搜索索引

**文章搜索索引 (article_search)**
| 字段名 | 数据类型 | 说明 |
|--------|----------|------|
| id | keyword | 主键 |
| title | text | 文章标题 |
| content | text | 文章内容 |
| tags | keyword | 文章标签 |
| author | keyword | 作者 |
| created_date | date | 创建时间 |
| likenum | int32 | 点赞数 |
| head | int32 | 浏览量 |
| replynum | int32 | 回复数 |
| state | string | 状态(0:不可见 1:可见) |

**用户搜索索引 (user_search)**
| 字段名 | 数据类型 | 说明 |
|--------|----------|------|
| id | keyword | 主键 |
| user_id | text | 用户ID |
| user_name | text | 用户名 |
| email | keyword | 邮箱 |
| avatar_url | keyword | 头像URL |
| level | keyword | 用户等级 |
| points | integer | 用户积分 |
| last_login | date | 最后登录 |
| state | string | 状态(0:不可见 1:可见) |

### MongoDB 数据库

#### 博客库 (hip_blogs)

**文章内容表 (articles_content)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | ObjectId | 文章ID | |
| user_id | NumberLong | 作者ID | |
| title | String | 文章标题 | |
| content | String | 文章内容 | |
| head | NumberInt | 浏览量 | |
| like | NumberInt | 点赞量 | |
| cover_image | varchar(255) | 封面图片 | |
| tags | Array of Strings | 标签分类 | |
| is_public | tinyint | 公开状态 | 0:公开 1:不公开 |
| create_time | ISODate | 创建时间 | |
| update_time | ISODate | 修改时间 | |
| status | tinyint | 禁用状态 | 0:未禁止 1:禁止 |
| deleted_at | tinyint | 删除标志 | 0:未删除 1:删除 |

**文章评论表 (articles_comment)**
| 字段名 | 数据类型 | 说明 | 约束 |
|--------|----------|------|------|
| id | String | 评论ID | |
| articles_id | ObjectId | 文章ID | |
| content | String | 评论内容 | |
| user_id | String | 评论人ID | |
| nickname | String | 评论人名称 | |
| create_time | String | 评论日期 | |
| likenum | Int32 | 点赞数 | |
| replynum | Int32 | 回复数 | |
| state | String | 状态 | 0:不可见 1:可见 |
| parentid | String | 上级评论ID | 0:顶级评论 |

## 部署指南

### 环境准备
1. 安装 Docker
2. 确保服务器时间与本地时间一致

### 组件部署

**MySQL 8.3**
```bash
docker pull mysql:8.3
docker run --name hip_mysql --restart=always -v /home/hip/mysql:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:8.3
```

**RabbitMQ 3.9**
```bash
docker pull rabbitmq:3.9-management
docker run -d --name=rabbitmq --restart=always -p 5672:5672 -p 15672:15672 rabbitmq:3.9
# 启用延迟队列插件
docker cp rabbitmq_delayed_message_exchange-3.9.0.ez rabbitmq:/plugins
docker exec -it rabbitmq rabbitmq-plugins enable rabbitmq_delayed_message_exchange
docker restart rabbitmq
```

**Redis 7.0.13**
```bash
docker pull redis:7.0.13
docker run --name=hip_redis -d -p 6379:6379 --restart=always redis:7.0.13
```

**Nacos 2.3.1**
```bash
docker pull nacos/nacos-server:v2.3.1
docker run -d -e MODE=standalone -p 8848:8848 -p 9848:9848 -p 9849:9849 --name nacos2.3.1 --net=hip --restart=always nacos/nacos-server:v2.3.1
```

**MinIO**
```bash
docker pull minio/operator:v4.5.1
docker run -p 9000:9000 -p 9001:9001 --name=gmalldocker_minio -d --restart=always -e "MINIO_ROOT_USER=admin" -e "MINIO_ROOT_PASSWORD=admin123456" -v /home/data:/data -v /home/config:/root/.minio minio/operator:v4.5.1 server /data --console-address ":9001"
```

**MongoDB**
```bash
docker pull istio/examples-bookinfo-mongodb:1.19.1
mkdir -p /opt/mongo/data/db
docker run -d --restart=always -p 27017:27017 --name mongo -v /opt/mongo/data/db:/data/db istio/examples-bookinfo-mongodb:1.19.1
# 配置账号密码
docker exec -it mongo mongosh
use admin
db.createUser({user:"myroot",pwd:"123456",roles:["root"]})
```

**Elasticsearch 7.12.1**
```bash
docker load -i es.tar
docker run -d --name es -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "discovery.type=single-node" -v es-data:/usr/share/elasticsearch/data -v es-plugins:/usr/share/elasticsearch/plugins --privileged --network hm-net -p 9200:9200 -p 9300:9300 elasticsearch:7.12.1
# 安装IK分词器
docker volume inspect es-plugins
# 将IK插件解压到挂载目录的ik文件夹中
```

**Kibana 7.12.1**
```bash
docker load -i kibana.tar
docker run -d --name kibana -e ELASTICSEARCH_HOSTS=http://es:9200 --network=hm-net -p 5601:5601 kibana:7.12.1
```

**Seata 2.0.0**
```bash
docker pull seataio/seata-server:2.0.0
mkdir -p /opt/seata-config
# 创建registry.conf和file.conf配置文件
docker run -d -p 8091:8091 -p 7091:7091 --name seata-server seataio/seata-server:2.0.0
docker cp seata-server:/seata-server/resources/. /opt/seata/config
docker rm -f seata-server
# 修改/opt/seata/config/application.yml配置
docker run -d --name seata-server --restart=always -p 8091:8091 -p 7091:7091 -e SEATA_IP=<mysql_ip> -v /opt/seata/config:/seata-server/resources --net=hip seataio/seata-server:1.7.1
```

**Sentinel**
```bash
# 安装JDK17
tar -xvf jdk-17_linux-x64_bin.tar.gz
echo 'JAVA_HOME="/path/to/jdk-17"' >> /etc/environment
echo 'PATH="$JAVA_HOME/bin:$PATH"' >> /etc/environment
source /etc/environment

# 部署Sentinel
mkdir -p /usr/local/sentinel
nohup java -jar sentinel-dashboard-1.7.2.jar > /usr/local/sentinel/sentinel.log 2>&1 &
```

## 用户等级说明
- 3级及以上用户可以发布信息
- 6级用户可以申请管理员权限
- 统一账号可登录天启文苑系列所有项目
