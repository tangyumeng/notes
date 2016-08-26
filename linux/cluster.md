### LAMP

- scale on: 向上扩展
- scale out:向外扩展

### Load balanceing:
#### LB

- DNS LB 缺点
- Round Robin(轮询)

#### HA (high availability)

- [服务器如何实现承受如此大量的用户请求](https://www.zhihu.com/question/27629526)

rsync+inotify


DAS:Direct Attached storage
NAS:Network Attached storage


LB Cluster
负载均衡调度算法：rr，wrr
- [LVS集群之十种调度算法及负载均衡——理论](http://blog.csdn.net/scape1989/article/details/21085659)

Hardware:
-  F5,BIG IP
- citrix,NetScaler
- A10

Software

- 四层
  - LVS (iptables 不能同时使用)
- 七层
  - nginx
  - haproxy


- [Lvs之NAT、DR、TUN三种模式的应用配置案例](http://lansgg.blog.51cto.com/5675165/1229421)


LVS 持久连接：
