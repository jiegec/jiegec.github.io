# 在 LEDE（OpenWrt）上启用 wpad

WPAD（Web Proxy Auto-Discovery Protocol）是一个可以利用 dhcp 分发 pac 配置的协议。方法如下：

```shell
$ # ssh to router first
$ vim /etc/dnsmasq.conf
dhcp-option=252,"http://router_ip/wpad.dat"
$ vim /www/wpad.dat # put pac here
$ service dnsmasq restart
$ # ensure proxy is available to lan
$ # enable wpad on devices
```

参考文档：

1. [Web Proxy Auto-Discovery Protocol](https://en.wikipedia.org/wiki/Web_Proxy_Auto-Discovery_Protocol)
1. [Automatic Proxy Configuration with WPAD](https://www.davidpashley.com/articles/automatic-proxy-configuration-with-wpad/)
1. [Deployment Options](https://findproxyforurl.com/deployment-options/)
1. [Example PAC File](http://findproxyforurl.com/example-pac-file/)
