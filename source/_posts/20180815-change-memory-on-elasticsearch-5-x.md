---
title: ElasticSearch 5.x 메모리 크기 변경
tags:
  - ElasticSearch
categories:
  - Programming
date: 2018-08-15 14:57:28
updated:
comments:
---

필자는 ElasticSearch를 사용하려고 준비중이에요. 그렇다고 너무 큰 인스턴스를 사용하기에는 낭비라는 생각이 들어 g1 인스턴스에 설치를 하고 있어요.

기본적으로 ElasticSearch는 `-Xms2G -Xmx2G`로 돌아가요. 기본 할당 메모리가 무려 2GB나 되어요. 그러니 1.7GB밖에 없는 g1 인스턴스에서 실행시키니 오류가 나는건 당연하죠.

인터넷에서는 `/etc/elasticsearch/elasticsearch.yml` 파일이나 systemd의 `/etc/default/elasticsearch` 파일 같은걸 수정하라고 적혀있던데, 여기에 낚이면 안돼요. 우리가 수정해야 될 파일은 `/etc/elasticsearch/jvm.options`예요.

```
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms2g
-Xmx2g
```

위 코드를 찾아

```
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms1g
-Xmx1g
```

위 코드처럼 `1g`로 바꾼 후 `sudo -i service elasticsearch start`, `sudo -i service elasticsearch status`로 확인해 보면 정상적으로 실행되어 있는 것을 확인할 수 있어요.

```bash
ryush00@elasticsearch:~$ sudo -i service elasticsearch status
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; disabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-08-15 05:43:06 UTC; 8min ago
     Docs: http://www.elastic.co
  Process: 9232 ExecStartPre=/usr/share/elasticsearch/bin/elasticsearch-systemd-pre-exec (code=exited, status=0/SUCCESS)
 Main PID: 9240 (java)
    Tasks: 31 (limit: 1997)
   CGroup: /system.slice/elasticsearch.service
           └─9240 /usr/bin/java -Xms1g -Xmx1g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+AlwaysPreTouch -server -Xss1m -Dj

Aug 15 05:43:06 elasticsearch systemd[1]: Starting Elasticsearch...
Aug 15 05:43:06 elasticsearch systemd[1]: Started Elasticsearch.
lines 1-12/12 (END)
```

`curl 127.0.0.1:9200` 명령어를 통해 서버 상태를 확인해보면 정상적으로 ElasticSearch 서버가 켜져 있는걸 확인할 수 있어요.

```bash
ryush00@elasticsearch:~$ curl 127.0.0.1:9200
{
  "name" : "MmH-kul",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "*********************",
  "version" : {
    "number" : "5.6.10",
    "build_hash" : "b727a60",
    "build_date" : "2018-06-06T15:48:34.860Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```