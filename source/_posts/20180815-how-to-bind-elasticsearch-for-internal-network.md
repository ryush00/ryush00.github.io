---
title: ElasticSearch 내부 네트워크용으로 열기
tags:
  - null
categories:
  - null
date: 2018-08-15 15:09:33
updated:
comments:
---

당연하겠지만, ElasticSearch는 내부에서만 접속이 가능해야 해요. 물론 비밀번호 걸고 암호화하고 외부로 열면 쓸 수는 있겠지만 흔하지는 않을거에요.

내부에서만 접속이 가능하게 하기 위해서 bind된 아이피를 바꿔줘야해요. 기본적으로 ElasticSearch는 127.0.0.1로 bind되어 있어요. 외부에서는 접속할 수 없죠. 이 값을 내부 네트워크의 내 아이피로 바꿔줘야 내부에서만 접속이 가능하게 될거예요.

`/etc/elasticsearch/elasticsearch.yml` 파일을 열어서 아래와 같은 줄들을 찾아서 `network.host`의 주석을 지우고 내부 아이피를 적어요. (`http.port`를 변경하면 포트를 변경할 수 있어요.)

```yaml
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 10.140.0.5
#
# Set a custom port for HTTP:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
```

그리고 elasticsearch를 재시작하고 상태를 확인해요.

```bash
root@elasticsearch:/etc/elasticsearch# service elasticsearch restart
root@elasticsearch:/etc/elasticsearch# service elasticsearch status
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; disabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-08-15 06:15:21 UTC; 1s ago
     Docs: http://www.elastic.co
  Process: 9304 ExecStartPre=/usr/share/elasticsearch/bin/elasticsearch-systemd-pre-exec (code=exited, status=0/SUCCESS)
 Main PID: 1234 (java)
    Tasks: 13 (limit: 1997)
   CGroup: /system.slice/elasticsearch.service
           └─1234 /usr/bin/java -Xms1g -Xmx1g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+AlwaysPreTouch -server -Xss1m -Dj

Aug 15 06:15:21 elasticsearch systemd[1]: Stopped Elasticsearch.
Aug 15 06:15:21 elasticsearch systemd[1]: Starting Elasticsearch...
Aug 15 06:15:21 elasticsearch systemd[1]: Started Elasticsearch.
lines 1-13/13 (END)
```

그리고 내부 네트워크의 다른 서버에서 잘 접속이 되는지 확인하면 끝.

```bash
ryush00@dev:~$ curl 10.140.0.5:9200
{
  "name" : "MDH-kxl",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "********************",
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



