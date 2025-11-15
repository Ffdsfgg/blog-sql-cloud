# 博客微服务数据库设计

**PS:**本项目主要用于技术练习与架构探索，功能尚未完全实现，当前重点为数据库设计与微服务架构搭建。

# 技术栈

**后端**

|     |     |     |     |
| --- | --- | --- | --- |
| SpringCloud Alibaba( Nacos , Gateway , Sentinel , Seata ) |     |     |     |
| Graalvm17(jdk17) | SpringBoot | Ehcache | mybatis-plus |
| spring Aot | hutool | HikariCP | drools |
| knife4j(api文档) | Minio | xxl-job | Fastjson (json） |
| Redisson | Shiro | OAuth2 | Filebeat |
| RabbitMQ | Apache POI | Spring Aop | oss |

**前端**

**网站**：vue3，Element-plus，(three.th目前不是很了解，预计)

**手机**:uniapp

**Pc:**预计

**数据库**

|     |     |     |
| --- | --- | --- |
| mysql | redis | Mongodb |

# 数据库设计

**Mysql**

想法：如果一个用户注册过了那么他可以使用这个账号登录天启文苑系列的全部项目，后面在细分博客的配置和小说的配置(**这个想法不知道好不好当前是这样子设计的)**

**库**（hip_system）用户

**Ps:想要发部信息等级至少3级，6级可以申请当管理员**

|     |     |     |     |
| --- | --- | --- | --- |
| 表(admin_info)用户信息 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | bigint | id  | 自增 ,非空 |
| Admin_id | bigint | 管理员id |     |
| username | varchar (100) | 用户名称 | 非空  |
| password | varchar (100) | 用户密码 | 非空  |
| User_id | bigint | 用户id |     |
| Create_time | datetime | 创建时间 |     |
| update_time | datetime | 修改时间 |     |
| create_user | bigint | 创建人id |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表(sys_permission)用户权限 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | bigint | id  | 自增 ,非空 |
| role_id | int | 角色id |     |
| benfits | varchar (100) | 角色所拥有的权限 | 非空  |
| permission_remark | varchar (100) | 权限描述 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表(sys_role)用户角色 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | bigint | id  | 自增 ,非空 |
| role_id | int | 角色id |     |
| role_name | varchar (20) | 角色名称 | 非空  |
| required_points | Int | 所需要的积分 |     |
| create_time | Datetime | 创建时间 |     |
| update_time | Datetime | 最后修改时间 |     |
| is_deleted | tinyint | 0：未删除 1：删除 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表(sys_user)用户表 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | bigint | id  | 自增 ,非空 |
| user_id | varchar(50) | 用户账号 |     |
| openid | varchar (50) | 微信用户的唯一标识 | 非空  |
| username | varchar(50) | 用户名称 |     |
| password | varchar(100) | 用户密码 |     |
| email | varchar(100) | 电子邮箱 |     |
| phone | varchar(15) | 手机号 |     |
| create_time | Datetime | 创建时间 |     |
| update_time | Datetime | 最后修改时间 |     |
| is_deleted | tinyint | 0：未删除 1：删除 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表(sys_user_role)用户权限 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | bigint | id  | 自增 ,非空 |
| Role_id | int | 角色id |     |
| user_id | int | 用户id |     |
| role_category | int | 角色类别 |     |
| create_time | datetime | 创建时间 |     |
| update_time | datetime | 修改时间 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表(user_login_log)用户登录 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | Int | id  | 自增 ,非空 |
| user_id | int | 用户id |     |
| status | tinyint | 登录状态（0失败，1成功） |     |
| msg | varchar(255) | 提示信息如：网站，微信 |     |
| message | varchar(500) | 额外描述信息 |     |
| login_time | datetime | 创建时间 |     |
| ipaddr | varchar(128) | 登录ip地址 |     |

**库**（hip_payment支付表）

|     |     |     |     |
| --- | --- | --- | --- |
| 表（integral）支付表 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | Int | 编号  | 自增 ,非空 |
| user_id | bigint | 用户id |     |
| order_id | Int | 订单ID |     |
| amount | declmal | 支付金额 |     |
| payment_method | Varchar(20) | 支付方式 |     |
| status | tinyint(3) | 支付状态 | 0支付成功1失败 |
| message | Text | 额外内容 |     |
| Create_time | datetime | 创建时间 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（user_ integral）用户积分 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | int | 积分id | 自增 ,非空 |
| user_id | bigint | 用户id |     |
| integral_change | DECIMAL | 积分变动的数量 |     |
| current_integral | DECIMAL | 当前积分总数 |     |
| Create_time | DATETIME | 创建时间 |     |

**库**（hip_blogs）博客

|     |     |     |     |
| --- | --- | --- | --- |
| 表（not_history）历史记录 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | Int | 历史记录id |     |
| user_id | bigint | 用户id | 逻辑外键 |
| articles_id | bigint | 博客id | 逻辑外键 |
| watch_time | datetime | 观看时间 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（tags）标签 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | Int | 标签id |     |
| tags_name | Varchar(20) | 标签名称 |     |
| introduce | Varchar(255) | 标签介绍 |     |
| deleted_at | tinyint(3) | 标签删除 | 0未删除，1删除 |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（blog_tags）博客标签 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | Int | 标签id |     |
| blog_id | Int | 博客id |     |
| tabs | int) | 标签id |     |
| deleted_at | tinyint(3) | 文章删除 | 0未删除，1删除 |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（attention_user）关注用户 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | int | 关注id |     |
| Subscriber_id | bigint | 关注者id |     |
| no_subscriber_id | Bigint | 被关注者id |     |
| subscriber_time | datetime | 关注时间 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（attention_user_log）关注用户日志 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | int | 关注id |     |
| Subscriber_id | bigint | 关注者id |     |
| no_subscriber_id | Bigint | 被关注者id |     |
| subscriber_time | datetime | 关注时间 |     |
| status | tinyint (3) | 操作状态 | 0删除，1关注 |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（blacklist_user ）黑名单用户 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | int | 黑名单id |     |
| blocker_id | bigint | 黑名单id |     |
| no_blocked_id | Bigint | 被屏蔽者 |     |
| blocked \_time | datetime | 屏蔽时间 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（ attention_collect）收藏文章 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | int | 收藏id |     |
| collect_id | bigint | 收藏者 |     |
| attention_id | Bigint | 文章id |     |
| collect_time | datetime | 收藏时间 |     |
| image_url | varchar(255) | 封面图片 |     |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（blacklist_ collect ）黑名单文章 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | int | 黑名单id |     |
| blocker_id | bigint | 黑名单者 |     |
| no_blocked | Bigint | 被屏蔽文章 |     |
| blocked_time | datetime | 屏蔽时间 |     |

**Elasticsearch修改为Lucene**

|     |     |     |     |
| --- | --- | --- | --- |
| （article_search）文章搜索 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | keyword | 主   |     |
| title | text | 文章标题 |     |
| content | Text | 文章内容 |     |
| tags | keyword | 文章标签 |     |
| author | keyword | 作者  |     |
| created_date | date | 创建时间 |     |
| likenum | Int32 | 点赞数 |     |
| Head | Int32 | 浏览量 |     |
| replynum | Int32 | 回复数 |     |
| state | String | 状态  | 0不可见1可见 |

|     |     |     |     |
| --- | --- | --- | --- |
| （user_search）用户搜索 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | keyword | 主   |     |
| user_id | text | 用户id |     |
| user_name | text | username |     |
| email | keyword | 邮箱  |     |
| avatar_url | keyword | 头像url |     |
| level | keyword | 用户当前等级 |     |
| points | integer | 用户当前积分 |     |
| last_login | date | 最后登录时间 |     |
| state | String | 状态  | 0不可见1可见 |

**Mongodb**

**库**（hip_blogs）博客

|     |     |     |     |
| --- | --- | --- | --- |
| 表（articles_centent）文章内容 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| id  | ObjectId | 文章id |     |
| user_id | NumberLong | 作者id |     |
| title | String | 文章标题 |     |
| content | String | 文章内容 |     |
| Head | NumberInt | 浏览量 |     |
| like | NumberInt | 点赞量 |     |
| cover_image | varchar(255) | 封面图片 |     |
| tags | Array of Strings | 标签分类 |     |
| is_public | tinyint (3) | 是否公开 | 0公开，1不公开 |
| create_time | ISODate | 文章创建时间 |     |
| Update_time | ISODate | 文章修改时间 |     |
| status | tinyint(3) | 文章禁用 | 0未禁止，1禁止 |
| deleted_at | tinyint(3) | 文章删除 | 0未删除，1删除 |

|     |     |     |     |
| --- | --- | --- | --- |
| 表（articles_comment）文章评论 |     |     |     |
| **字段名** | **数据类型** | **说明** | **备注** |
| Id  | String | 评论id |     |
| Artickes_id | ObjectId | 文章id |     |
| content | String | 评论内容 |     |
| User_id | String | 评论人id |     |
| Niclname | String | 评论人名称 |     |
| Create_time | String | 评论日期 |     |
| likenum | Int32 | 点赞数 |     |
| replynum | Int32 | 回复数 |     |
| state | String | 状态  | 0不可见1可见 |
| parentid | 上级ID | String | 如果为0表示文章的顶级评论 |

# 部署

**1安装docker**

**2部署mysql：**

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/mysql:8.3

运行：sudo docker run --name hip_mysql --restart=always -v /home/hip/mysql:/var/lib/mysql -p 3306:3306 -e MYSQL\\\_ROOT\\\_PASSWORD=root -d （mysql:8.3）

验证：

进入容器：sudo docker exec -it hip_mysql /bin/bash

登录mysql：mysql -u root -p root

**3部署rabbitmq**：

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/library/rabbitmq:3.9-management

运行：sudo docker run -d --name=rabbitmq --restart=always -p 5672:5672 -p 15672:15672 （rabbitmq:3.9）

安装延迟队列插件 ：

先把插件导入服务器里面 sudo docker cp rabbitmq_delayed_message_exchange-3.9.0.ez rabbitmq:/plugins

进入容器里面：sudo docker exec -it rabbitmq /bin/bash

进入 plugins里面：cd plugins 查看有没有，如何有rabbitmq_delayed_message_exchange-3.9.0.ez就成功了

在容器内plugins目录下，执行 rabbitmq-plugins enable rabbitmq_delayed_message_exchange 命令启用插件

退出RabbitMQ容器内部，然后执行sudo docker restart rabbitmq 命令重启RabbitMQ容器

**4部署redis：**

拉取： sudo docker pull hub.atomgit.com/amd64/redis:7.0.13

运行：sudo docker run --name=hip_redis -d -p 6379:6379 --restart=always (redis：7.0.13)

**5部署nacos：**

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/nacos/nacos-server:v2.3.1

运行：sudo docker run -d -e MODE=standalone -p 8848:8848 -p 9848:9848 -p 9849:9849 --name nacos2.3.1 --net=hip --restart=always (nacos2.3.1)

**6部署minio：**

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/minio/operator:v4.5.1

运行：docker run -p 9000:9000 -p 9001:9001 --name=gmalldocker_minio -d --restart=always -e "MINIO_ROOT_USER=admin" -e "MINIO_ROOT_PASSWORD=admin123456" -v /home/data:/data -v /home/config:/root/.minio （1338dbbe2aeb） server /data --console-address ":9001"

**7部署mongodb：**

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/istio/examples-bookinfo-mongodb:1.19.1

创建和启动容器需要在宿主机建立文件夹rm -rf /opt/mongo和mkdir -p /opt/mongo/data/db

运行：sudo docker run -d --restart=always -p 27017:27017 --name mongo -v /opt/mongo/data/db:/data/db （f5067358b009）

后面要不要配置账号密码看自己需求（本人建议配置）

设置账号密码：

进入容器：docker exec -it mongo mongosh

配置文档：cd/mongodb/single/mongod.conf

切换：use admin表

注册：db.createUser({user:"myroot",pwd:"123456",roles:\["root"\]})

**8部署Elasticsearch**

拉取：因为该文件已经给了所以（sudo docker load -i es.tar ）

运行：sudo docker run -d --name es -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "discovery.type=single-node" -v es-data:/usr/share/elasticsearch/data -v es-plugins:/usr/share/elasticsearch/plugins --privileged --network hm-net -p 9200:9200 -p 9300:9300 elasticsearch:7.12.1

安装ik插件：

先查找：sudo docker volume inspect es-plugins

获得 {

"CreatedAt": "2024-08-29T16:33:32+08:00",

"Driver": "local",

"Labels": null,

"Mountpoint": "/var/lib/docker/volumes/es-plugins/\_data",

"Name": "es-plugins",

"Options": null,

"Scope": "local"

}

根据Mountpoint给的路径进入然后权限不够就sudo su

新建一个文件夹ik

解压我给的文件夹elasticsearch-analysis-ik-7.12.1放入里面ik里面

最后重新启动

**9部署kibana：**

拉取：因为该文件已经给了所以（sudo docker load -i kibana.tar ）

运行：sudo docker run -d --name kibana -e ELASTICSEARCH_HOSTS=http://es:9200 --network=hm-net -p 5601:5601 kibana:7.12.1

**10部署seata**

拉取：sudo docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/seataio/seata-server:2.0.0

新建配置类：在opt下创建seata-config目录，并在该目录下边新建registry.conf、file.conf文件。

运行：sudo docker run -d -p 8091:8091 -p 7091:7091 --name seata-server （seataio/seata-server:2.0.0）

拉配置：docker cp seata-server:/seata-server/resources/. /opt/seata/config

完成之后删除临时容器(过河拆桥)：sudo docker rm -f seata-server

去/opt/seata/config/application.yml改配置：配置直接找

运行：sudo docker run -d --name seata-server --restart=always -p 8091:8091 -p 7091:7091 -e SEATA_IP=（mysqlip） -v /opt/seata/config:/seata-server/resources --net=hip（seataio/seata-server:1.7.1）

解压：tar -zxvf seata-server-2.0.0.tar.gz

进入到conf里面修改application.yml配置：配置放文件里面了

**11部署 Sentinel ：**

安装jdk17：

解压：tar -xvf jdk-17_linux-x64_bin.tar.gz

配置环境变量：sudo vim /etc/environment

在后面加：JAVA_HOME="/path/to/jdk-17"

PATH="$JAVA_HOME/bin:$PATH"

生效：source /etc/environment

将Sentinel复制到/usr/local/sentinel文件下

运行nohup java -jar （sentinel-dashboard-1.7.2.jar） > /usr/local/sentinel/sentinel.log 2>&1 &

PS:文件上传时，需要调整一下linux 服务器的时间与windows 时间一致！
