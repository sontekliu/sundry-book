# 01 ElasticSearch 安装

全文搜索目前已成为各个互联网公司常见的需求，ElasticSearch（以下简称ES）已成为目前市面上全文搜索引擎的首选。

ES 的官方网址是：https://www.elastic.co

> 注意，本文是基于 ES 6.7.0，操作系统是 macOS Mojave 10.14.3

### 1. ElasticSearch 安装

1.1 ES 的安装非常简单，首先从官方网站下载对应操作系统的软件包。

```shell
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.0.tar.gz
$ tar -zxvf elasticsearch-6.7.0
$ ls -l
total 896
drwxr-xr-x  45 sontek  staff    1440 Mar 21 23:34 bin
drwxr-xr-x   9 sontek  staff     288 Mar 21 23:34 config
drwxr-xr-x  42 sontek  staff    1344 Mar 21 23:34 lib
drwxr-xr-x   2 sontek  staff      64 Mar 21 23:34 logs
drwxr-xr-x  31 sontek  staff     992 Mar 21 23:34 modules
drwxr-xr-x   2 sontek  staff      64 Mar 21 23:34 plugins
-rw-r--r--   1 sontek  staff   13675 Mar 21 23:29 LICENSE.txt
-rw-r--r--   1 sontek  staff  427502 Mar 21 23:34 NOTICE.txt
-rw-r--r--   1 sontek  staff    8519 Mar 21 23:29 README.textile
```

1.2 启动 ES

```shell
$ ./bin/elasticsearch
```

如果一切正常，那么 ES 就会在默认的 9200 端口运行，此时打开浏览器可请求该端口。或者使用 curl 命令也可以

```shell
$ curl localhost:9200
{
  "name" : "ctHNbGD",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "cjtHUz12RViYzSua9a0uIw",
  "version" : {
    "number" : "6.7.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "8453f77",
    "build_date" : "2019-03-21T15:32:29.844721Z",
    "build_snapshot" : false,
    "lucene_version" : "7.7.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

在上面的代码中，ES 返回一个 JSON 对象，其中包含当前节点、集群、版本等信息。

默认情况下，ES 只允许本机进行访问，如需要远程访问，需修改 ES 配置文件。将 ES 安装目录下的 `config/elasticsearch.yml` 文件中的
`network.host` 注释去掉，并将其值改成 `0.0.0.0`，然后重新启动即可。

```shell
network.host: 0.0.0.0
```
