# 高可用

## 方案

* redis多副本（主从复制）
* redis sentinel（哨兵）
* redis cluster（集群）

```mermaid
graph TB;
  id1((master)) --> id2((slave1))
  id1((master))--> id3((slave2))
```
