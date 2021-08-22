## 一、ELK介绍

### 1.1、ELK简介（https://www.cnblogs.com/zsql/p/13164414.html#_label1）

​     ELK是Elasticsearch、Logstash、Kibana三大开源框架首字母大写简称(但是后期出现的filebeat(beats中的一种)可以用来替代logstash的数据收集功能，比较轻量级)。市面上也被成为Elastic Stack。

​    Filebeat是用于转发和集中日志数据的轻量级传送工具。Filebeat监视您指定的日志文件或位置，收集日志事件，并将它们转发到Elasticsearch或 Logstash进行索引。Filebeat的工作方式如下：启动Filebeat时，它将启动一个或多个输入，这些输入将在为日志数据指定的位置中查找。对于Filebeat所找到的每个日志，Filebeat都会启动收集器。每个收集器都读取单个日志以获取新内容，并将新日志数据发送到libbeat，libbeat将聚集事件，并将聚集的数据发送到为Filebeat配置的输出。

​    Logstash是免费且开放的服务器端数据处理管道，能够从多个来源采集数据，转换数据，然后将数据发送到您最喜欢的“存储库”中。Logstash能够动态地采集、转换和传输数据，不受格式或复杂度的影响。利用Grok从非结构化数据中派生出结构，从IP地址解码出地理坐标，匿名化或排除敏感字段，并简化整体处理过程。

​	Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana，可以通过各种图表进行高级数据分析及展示。并且可以为 Logstash 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以汇总、分析和搜索重要数据日志。还可以让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板（dashboard）实时显示Elasticsearch查询动态

### 1.2、为什么要使用ELK

​	日志主要包括系统日志、应用程序日志和安全日志。系统运维和开发人员可以通过日志了解服务器软硬件信息、检查配置过程中的错误及错误发生的原因。经常分析日志可以了解服务器的负荷，性能安全性，从而及时采取措施纠正错误。

​	往往单台机器的日志我们使用grep、awk等工具就能基本实现简单分析，但是当日志被分散的储存不同的设备上。如果你管理数十上百台服务器，你还在使用依次登录每台机器的传统方法查阅日志。这样是不是感觉很繁琐和效率低下。当务之急我们使用集中化的日志管理，例如：开源的syslog，将所有服务器上的日志收集汇总。集中化管理日志后，日志的统计和检索又成为一件比较麻烦的事情，一般我们使用grep、awk和wc等Linux命令能实现检索和统计，但是对于要求更高的查询、排序和统计等要求和庞大的机器数量依然使用这样的方法难免有点力不从心。

​	一般大型系统是一个分布式部署的架构，不同的服务模块部署在不同的服务器上，问题出现时，大部分情况需要根据问题暴露的关键信息，定位到具体的服务器和服务模块，构建一套集中式日志系统，可以提高定位问题的效率。

### 1.3、完整日志系统基本特征

- 收集：能够采集多种来源的日志数据
- 传输：能够稳定的把日志数据解析过滤并传输到存储系统
- 存储：存储日志数据
- 分析：支持 UI 分析
- 警告：能够提供错误报告，监控机制

## 二、ELK架构分析

### 2.1、beats+elasticsearch+kibana模式

![image-20210513205726289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513205726289.png)

如上图所示，该ELK框架由beats（日志分析我们通常使用filebeat）+elasticsearch+kibana构成，这个框架比较简单，入门级的框架。其中filebeat也能通过module对日志进行简单的解析和索引。并查看预建的Kibana仪表板。生产环境建议使用logstash

### 2.2、beats+logstash+elasticsearch+kibana模式

![image-20210513210044659](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513210044659.png)

该框架是在上面的框架的基础上引入了logstash，引入logstash带来的好处如下：

- 通Logstash具有基于磁盘的自适应缓冲系统，该系统将吸收传入的吞吐量，从而减轻背压
- 从其他数据源（例如数据库，S3或消息传递队列）中提取
- 将数据发送到多个目的地，例如S3，HDFS或写入文件
- 使用条件数据流逻辑组成更复杂的处理管道

filebeat结合logstash带来的优势：

​	1、水平可扩展性，高可用性和可变负载处理：filebeat和logstash可以实现节点之间的负载均衡，多个logstash可以实现logstash的高可用

​	2、消息持久性与至少一次交付保证：使用Filebeat或Winlogbeat进行日志收集时，可以保证至少一次交付。从Filebeat或Winlogbeat到Logstash以及从Logstash到Elasticsearch的两种通信协议都是同步的，并且支持确认。Logstash持久队列提供跨节点故障的保护。对于Logstash中的磁盘级弹性，确保磁盘冗余非常重要。

​	3、具有身份验证和有线加密的端到端安全传输：从Beats到Logstash以及从 Logstash到Elasticsearch的传输都可以使用加密方式传递 。与Elasticsearch进行通讯时，有很多安全选项，包括基本身份验证，TLS，PKI，LDAP，AD和其他自定义领域

当然在该框架的基础上还可以引入其他的输入数据的方式：比如：TCP，UDP和HTTP协议是将数据输入Logstash的常用方法（如下图所示）：

![image-20210513210745099](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513210745099.png)

### 2.3、beats+缓存/消息队列+logstash+elasticsearch+kibana模式

![image-20210513210845021](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513210845021.png)

在如上的基础上我们可以在beats和logstash中间添加一些组件redis、kafka、RabbitMQ等，添加中间件将会有如下好处：
第一，降低对日志所在机器的影响，这些机器上一般都部署着反向代理或应用服务，本身负载就很重了，所以尽可能的在这些机器上少做事；
第二，如果有很多台机器需要做日志收集，那么让每台机器都向Elasticsearch持续写入数据，必然会对Elasticsearch造成压力，因此需要对数据进行缓冲，同时，这样的缓冲也可以一定程度的保护数据不丢失；
第三，将日志数据的格式化与处理放到Indexer中统一做，可以在一处修改代码、部署，避免需要到多台机器上去修改配置

## 三、ELK部署 

### 3.1、filebeat的安装介绍

#### 3.1.1、原理

​	Filebeat的工作方式如下：启动Filebeat时，它将启动一个或多个输入，这些输入将在为日志数据指定的位置中查找。对于Filebeat所找到的每个日志，Filebeat都会启动收集器。每个收集器都读取单个日志以获取新内容，并将新日志数据发送到libbeat，libbeat将聚集事件，并将聚集的数据发送到为Filebeat配置的输出

​	Filebeat结构：由两个组件构成，分别是inputs（输入）和harvesters（收集器），这些组件一起工作来跟踪文件并将事件数据发送到您指定的输出，harvester负责读取单个文件的内容。harvester逐行读取每个文件，并将内容发送到输出。为每个文件启动一个harvester。harvester负责打开和关闭文件，这意味着文件描述符在harvester运行时保持打开状态。如果在收集文件时删除或重命名文件，Filebeat将继续读取该文件。这样做的副作用是，磁盘上的空间一直保留到harvester关闭。默认情况下，Filebeat保持文件打开，直到达到close_inactive

#### 3.1.2、简单安装

本文采用压缩包的方式安装，linux版本，filebeat-7.7.0-linux-x86_64.tar.gz

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.0-linux-x86_64.tar.gz
tar -xzvf filebeat-7.7.0-linux-x86_64.tar.gz
```

配置示例文件：filebeat.reference.yml（包含所有未过时的配置项）
配置文件：filebeat.yml
启动命令：./filebeat -e

### 3.2、logstash的安装介绍

#### 3.2.1、基本原理

logstash分为三个步骤：inputs（必须的）→ filters（可选的）→ outputs（必须的），inputs生成时间，filters对其事件进行过滤和处理，outputs输出到输出端或者决定其存储在哪些组件里。inputs和outputs支持编码和解码

Logstash管道中的每个input阶段都在自己的线程中运行。将写事件输入到内存（默认）或磁盘上的中心队列。每个管道工作线程从该队列中取出一批事件，通过配置的filter处理该批事件，然后通过output输出到指定的组件存储。管道处理数据量的大小和管道工作线程的数量是可配置的

数据集我们采用apache的日志格式，下载地址：https://download.elastic.co/demos/logstash/gettingstarted/logstash-tutorial.log.gz

#### 3.2.2、简单安装

下载地址1：https://www.elastic.co/cn/downloads/logstash  

past releases 可以下载到历史版本 ELK三个版本最好对应起来

解压即安装

```
tar -zxvf logstash-7.7.0.tar.gz -C /usr/elk
cd /usr/elk/logstash-7.7.0
```

我们先来一个简单的案例：

```
bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

启动 Logstash 后，再键入 Hello World，结果如下：

```
[root@localhost logstash-5.6.3]# bin/logstash -e 'input { stdin { } } output { stdout {} }'
Sending Logstash's logs to /usr/logstash-5.6.3/logs which is now configured via log4j2.properties
[2017-10-27T00:17:43,438][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"fb_apache", :directory=>"/usr/logstash-5.6.3/modules/fb_apache/configuration"}
[2017-10-27T00:17:43,440][INFO ][logstash.modules.scaffold] Initializing module {:module_name=>"netflow", :directory=>"/usr/logstash-5.6.3/modules/netflow/configuration"}
[2017-10-27T00:17:43,701][INFO ][logstash.pipeline        ] Starting pipeline {"id"=>"main", "pipeline.workers"=>1, "pipeline.batch.size"=>125, "pipeline.batch.delay"=>5, "pipeline.max_inflight"=>125}
[2017-10-27T00:17:43,744][INFO ][logstash.pipeline        ] Pipeline main started
The stdin plugin is now waiting for input:
[2017-10-27T00:17:43,805][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
Hello World
2017-10-27T07:17:51.034Z localhost.localdomain Hello World
```

在生产环境中，Logstash 的管道要复杂很多，可能需要配置多个输入、过滤器和输出插件。

因此，需要一个配置文件管理输入、过滤器和输出相关的配置。配置文件内容格式如下：

```
#　输入
input {
  ...
}

# 过滤器
filter {
  ...
}

# 输出
output {
  ...
}
```

#### 3.2.3、插件用法

在使用插件之前，我们先了解一个概念：事件。

Logstash 每读取一次数据的行为叫做事件。

在 Logstach 目录中创建一个配置文件，名为 logstash.conf（名字任意）。

##### 3.2.3.1输入插件

输入插件允许一个特定的事件源可以读取到 Logstash 管道中，配置在 input {} 中，且可以设置多个。

修改配置文件：

```
input {
    # 从文件读取日志信息
    file {
        path => "/usr/elk/logstash-tutorial"
        type => "system"
        start_position => "beginning"
    }
}

# filter {
#
# }

output {
    # 标准输出
    stdout { codec => rubydebug }
}
```

在控制台结果如下：

```
{
      "@version" => "1",
          "host" => "localhost.localdomain",
          "path" => "/var/log/messages",
    "@timestamp" => 2017-10-29T07:30:02.601Z,
       "message" => "Oct 29 00:30:01 localhost systemd: Starting Session 16 of user root.",
          "type" => "system"
}
....
```

##### 3.2.3.2输出插件

输出插件将事件数据发送到特定的目的地，配置在 output {} 中，且可以设置多个。

修改配置文件：

```
input {
    # 从文件读取日志信息
    file {
        path => "/usr/elk/logstash-tutorial"
        type => "system"
        start_position => "beginning"
    }
    
}

# filter {
#
# }

output {
    # 输出到 elasticsearch
    elasticsearch {
        hosts => ["192.168.5.128:9200"]
        index => "error-%{+YYYY.MM.dd}"
    }
}
```

配置文件中使用 elasticsearch 输出插件。输出的日志信息将被保存到 Elasticsearch 中，索引名称为 index 参数设置的格式。

##### 3.2.3.3编码解码插件

Logstash 默认没有安装该插件，需要开发者自行安装。键入：

```
bin/logstash-plugin install logstash-codec-multiline
```

修改配置文件：

```
input {
    # 从文件读取日志信息
    file {
        path => "/var/log/error.log"
        type => "error"
        start_position => "beginning"
        # 使用 multiline 插件
        codec => multiline {
            # 通过正则表达式匹配，具体配置根据自身实际情况而定
            pattern => "^\d"
            negate => true
            what => "previous"
        }
    }

}

# filter {
#
# }

output {
    # 输出到 elasticsearch
    elasticsearch {
        hosts => ["192.168.2.41:9200"]
        index => "error-%{+YYYY.MM.dd}"
    }
}
```

##### 3.2.3.4过滤器插件

过滤器插件位于 Logstash 管道的中间位置，对事件执行过滤处理，配置在 filter {}，且可以配置多个。

本次测试使用 grok 插件演示，grok 插件用于过滤杂乱的内容，将其结构化，增加可读性。

安装：

```
bin/logstash-plugin install logstash-filter-grok
```

修改配置文件：

```
input {
     stdin {}
}


filter {
     grok {
       match => { "message" => "%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER
:duration}" }
     }
}


output {
     stdout {
        codec => "rubydebug"
     }
}
```

保存文件。键入：

```
bin/logstash -f logstash.conf
```

启动成功后，我们输入：

```
55.3.244.1 GET /index.html 15824 0.043
```

控制台返回：

```
55.3.244.1 GET /index.html 15824 0.043
{
      "duration" => "0.043",
       "request" => "/index.html",
    "@timestamp" => 2017-10-30T15:23:23.912Z,
        "method" => "GET",
         "bytes" => "15824",
      "@version" => "1",
          "host" => "localhost.localdomain",
        "client" => "55.3.244.1",
       "message" => "55.3.244.1 GET /index.html 15824 0.043"
}
```

### 3.3、elasticsearch的安装介绍

#### 3.3.1、基本介绍

​	Elasticsearch（ES）是一个基于Lucene构建的开源、分布式、RESTful接口的全文搜索引擎。Elasticsearch还是一个分布式文档数据库，其中每个字段均可被索引，而且每个字段的数据均可被搜索，ES能够横向扩展至数以百计的服务器存储以及处理PB级的数据。可以在极短的时间内存储、搜索和分析大量的数据。

​	基本概念有：Cluster 集群、Node节点、Index索引、Document文档、Shards & Replicas分片与副本等

elasticsearch的优势：

- 分布式：横向扩展非常灵活
- 全文检索：基于lucene的强大的全文检索能力；
- 近实时搜索和分析：数据进入ES，可达到近实时搜索，还可进行聚合分析
- 高可用：容错机制，自动发现新的或失败的节点，重组和重新平衡数据
- 模式自由：ES的动态mapping机制可以自动检测数据的结构和类型，创建索引并使数据可搜索。
- RESTful API：JSON + HTTP

#### 3.3.2、linux系统参数设置	

```
1.配置文件描述符修改
ulimit -n 65535  #临时修改
vim /etc/security/limits.conf #永久修改，对所有用户
*        soft    nproc     65535
*        hard    nproc     65535

2配置虚拟内存
vim /etc/sysctl.conf
vm.max_map_count=262144
保存，并执行 sysctl -p。
3.新增用户
#增加 es 组
groupadd liaole

#增加 es 用户并附加到 es 组
useradd liaole -g liaole -p 123456(密码)

#给目录权限
chown -R liaole:liaole elasticsearch-7.7.0

#使用es用户
su es

```

#### 3.3.3、安装

##### 3.3.3.1解压即安装

```
tar -zxvf elasticsearch-7.7.0.tar.gz -C /usr/elk

cd /usr/elk/elasticsearch-7.7.0

```

elasticsearch 文件目录如下：

```
$ES_HOME：/data/hd05/elk/elasticsearch-7.7.0
bin: $ES_HOME/bin  #es启动命令和插件安装命令
conf：$ES_HOME/conf #elasticsearch.yml配置文件目录
data：$ES_HOME/data  #对应的参数path.data，用于存放索引分片数据文件
logs：$ES_HOME/logs  #对应的参数path.logs，用于存放日志
jdk：$ES_HOME/jdk  #自带支持该es版本的jdk
plugins： $ES_HOME/jplugins #插件存放目录
lib： $ES_HOME/lib #存放依赖包，比如java类库
modules： $ES_HOME/modules #包含所有的es模块
```

##### 3.3.3.2 修改如下配置

```
cluster.name: my-application
node.name: node-1
path.data: /usr/elk/elasticsearch-7.7.0/data
path.logs: /usr/elk/elasticsearch-7.7.0/logs
network.host: 0.0.0.0 #允许远程机器访问
http.port: 9200 端口 对外的
cluster.initial_master_nodes: ["node-1"]
http.cors.enabled: true
http.cors.allow-origin: "*"  //允许跨域访问 ，允许其他插件访问

```

##### 3.3.3.3启动

```
bin/elasticsearch 或 bin/elasticsearch -d # -d 表示后台启动
```

##### 3.3.3.4 使用

Elasticsearch 支持 RESTFUL 风格 API，其 API 基本格式如下：

```
http://<ip>:<port>/<索引>/<类型>/<文档id>
```

##### 3.3.3.5 创建/删除索引

为了方便测试，我们使用 POSTMAN 工具进行接口的请求。

创建一个非结构化的索引，需要使用 **PUT** 请求。例如创建一个名为 book 的索引。

执行：

```
[PUT] http://192.168.2.41:9200/book
```

返回结果：

```
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "book"
}
```

删除一个索引，需要使用 **DELETE** 请求。

```
[DELETE] http://192.168.2.41:9200/book
```

##### 3.3.3.6 数据操作

插入指定 ID 的数据，需要使用 **PUT** 请求。如下图：

```
http://192.168.2.41:9200/book/java/1
```

修改数据，需要使用 **POST** 请求，且 URL 需要添加 _update

```
[POST] http://192.168.2.41:9200/book/java/1/_update
```

请求参数（修改颜色）：

```
{
    "doc": {
        "color": "black"
    }
}
```

删除数据，需要使用 **DELETE** 请求。

```
[DELETE] http://192.168.2.41:9200/book/java/1
```

查询指定ID的数据，需要使用 **GET** 请求。

```
[GET] http://192.168.2.41:9200/book/java/1
```

条件查询，需要使用 **POST** 请求。

执行：

```
[POST] http://192.168.2.41:9200/book/java/_search
```

请求参数（查找 color = "green"）：

```
{
    "query": {
        "match":{
            "color": "green"
        }
    }
}
```

### 3.4、kibana的安装介绍

下载地址：https://elasticsearch.cn/download/

##### 3.4.1 下载解压安装

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.7.0-linux-x86_64.tar.gz
tar -zxvf kibana-7.7.0-linux-x86_64.tar.gz /usr/elk
cd usr/elk/kibana-7.7.0-linux-x86_64/

```

##### 3.4.2 修改配置文件

```
vim config/kibana.yml

# 将默认配置改成如下：

server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://192.168.5.128:9200"]
kibana.index: ".kibana"
```

##### 3.4.3 启动

```
bin/kibana

```

启动后打开浏览器访问 [http://192.168.5.128:5601](http://192.168.2.43:5601/) 浏览 kibana 界面：