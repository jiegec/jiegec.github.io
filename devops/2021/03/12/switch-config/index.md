# 常用交换机命令

## 背景

最近接触了 Cisco，DELL，Huawei，H3C，Ruijie 的网络设备，发现配置方式各有不同，故记录一下各个厂家的命令。

## Huawei

测试型号：S5320

### 保存配置

```text
<HUAWEI>save
The current configuration will be written to flash:/vrpcfg.zip.
Are you sure to continue?[Y/N]y
Now saving the current configuration to the slot 0....
Save the configuration successfully.
```

### 进入配置模式

```text
<HUAWEI> system-view
```

### 查看当前配置

```text
[HUAWEI] display current-configuration
```

### 查看 LLDP 邻居

```text
[HUAWEI]display lldp neighbor brief
```

### 查看 CDP 邻居

```text
[HUAWEI]display cdp neighbor brief
```

### 启用 LLDP

```text
[HUAWEI]lldp enable
```

### 启用 CDP

```text
[HUAWEI-XGigabitEthernet0/0/1]lldp compliance cdp txrx
```

### 启用只读 SNMPv1 community

```text
[HUAWEI]snmp-agent sys-info version all
Warning: This command may cause confliction in netconf status. Continue? [Y/N]:y
Warning: SNMPv1/SNMPv2c is not secure, and it is recommended to use SNMPv3.
[HUAWEI]snmp-agent community read [COMMUNITY NAME]
Warning: This command may cause confliction in netconf status. Continue? [Y/N]:y
```

### 启用 SNMP iso view

默认情况下 SNMP 会缺少一些标准的 MIB（比如 LLDP），可以打开 iso view：

```text
[HUAWEI]snmp-agent mib-view included iso-view iso
Warning: This command may cause confliction in netconf status. Continue? [Y/N]:y
[HUAWEI]snmp-agent community read [COMMUNITY NAME] mib-view iso-view
```

### 查看 ARP 表

```text
[HUAWEI]display arp
```

### ARPING

```text
[HUAWEI]arp send-packet X.X.X.X ffff-ffff-ffff interface Vlanif VLAN
```

### 启用 STP 协议

```text
[HUAWEI]stp enable
[HUAWEI]stp mode vbst
```

### 设置 NTP 服务器

```text
[HUAWEI]ntp-service unicast-server x.x.x.x
```

### 设置远程 syslog 服务器

```text
[HUAWEI]info-center loghost x.x.x.x
```

### 设置 LACP 链路聚合

```text
[HUAWEI-XGigabitEthernet0/0/1]eth-trunk 1
[HUAWEI-XGigabitEthernet0/0/2]eth-trunk 1
[HUAWEI]interface Eth-Trunk 1
[HUAWEI-Eth-Trunk1]mode lacp
```

## DELL

测试型号：N3048

### 保存配置

```text
console#copy running-config startup-config

This operation may take few minutes.
Management interfaces will not be available during this time.

Are you sure you want to save? (y/n) y

Configuration Saved!
```

### 进入配置模式

```text
console>enable
console# configure
```

### 查看当前配置

```text
console# show running-config
```

### 查看 LLDP 邻居

```text
console#show lldp remote-device all
```

### VLAN Trunk 配置

```text
console(config)#interface Gi1/0/1
console(config-if-Gi1/0/1)#switchport mode trunk
console(config-if-Gi1/0/1)#switchport trunk allowed vlan xxx,xxx-xxx
```

### VLAN Access 配置

```text
console(config)#interface Gi1/0/1
console(config-if-Gi1/0/1)#switchport mode access
console(config-if-Gi1/0/1)#switchport access vlan xxx
```

### 查看 VLAN 配置

```text
console#show vlan
```

### 批量配置 interface

```text
console(config)#interface range Gi1/0/1-4
```

### 启用 SSH 服务器

```text
console(config)#crypto key generate rsa
console(config)#crypto key generate dsa
console(config)#ip ssh server
```

### 启用 CDP(DELL 称之为 ISDP)

```text
console(config)#isdp enable
```

### 启用只读 SNMPv1 community

```text
console(config)#snmp-server community [COMMUNITY NAME] ro
```

### 设置 NTP 服务器

```text
console(config)#sntp unicast client enable
console(config)#sntp server x.x.x.x
```

### 设置 NTP 服务器

```text
console(config)#sntp unicast client enable
console(config)#sntp server x.x.x.x
```

### 设置 STP 协议

```text
console(config)#spanning-tree mode rapid-pvst
```

## H3C

### 进入配置模式

```text
<switch>system-view
System View: return to User View with Ctrl+Z.
[switch]
```

### 查看当前配置

```text
[switch]display current-configuration
```

### 查看 lldp 邻居

```text
[switch]display lldp neighbor-information
```

### 保存配置

```text
[switch]save
The current configuration will be written to the device. Are you sure? [Y/N]:y
Please input the file name(*.cfg)[flash:/startup.cfg]
(To leave the existing filename unchanged, press the enter key):y
The file name is invalid(does not end with .cfg).
```

### 批量配置 interface

```text
[switch]interface range GigabitEthernet 1/0/1 to GigabitEthernet 1/0/24
[switch-if-range]
```

### 查看 MAC 地址表

```text
[switch]show mac-address
```

### 打开 LLDP 和 CDP

```text
[switch]lldp global enable
[switch]lldp compliance cdp
```

### 升级固件

```text
<switch> tftp 1.2.3.4 get SWITCH_FIRMWARE.ipe
<switch> boot-loader file flash:/SWITCH_FIRMWARE.ipe all main
<switch> show boot
<switch> save
<switch> reboot
```

### 配置 NTP

```text
[switch] ntp enable
[switch] ntp unicast-server 1.2.3.4
```

### 配置远程日志

```text
[switch] logging loghost 1.2.3.4
```

## Mellanox

### 进入配置模式

```text
switch > enable
switch # configure terminal
switch (config) #
```

### 查看当前配置

```text
switch (config) # show running-config
```

### 查看 interface 状态

```text
switch (config) # show interfaces brief
```

### 查看以太网端口状态

```text
switch (config) # show interfaces ethernet status
```

### 查看 lldp 邻居

```text
switch (config) # show lldp remote
```

### 保存配置

```text
switch (config) # configuration write
```

### 批量配置 interface

```text
switch (config) # interface ethernet 1/1/1-1/1/4
switch (config interface ethernet 1/1/1-1/1/4) #
```

### 查看 MAC 地址表

```text
switch (config) # show mac-address-table
```

### 查看链路聚合状态

```text
switch (config) # show interfaces port-channel summary
```

### 把拆分的四个 SFP 口恢复成一个

```text
switch (config interface ethernet 1/1/1) # module-type qsfp
```

### 把一个 QSFP 口拆分成四个

```text
switch (config interface ethernet 1/1) # shutdown
switch (config interface ethernet 1/1) # module-type qsfp-split-4
```

### 设置链路聚合

```text
switch (config interface ethernet 1/1) # channel-group 1 mode active
switch (config interface ethernet 1/2) # channel-group 1 mode active
```

模式可以选择：active(LACP)/passive(LACP)/on(Static)

### 设置 STP 协议

```text
switch (config) # spanning-tree mode rpvst
```

### 设置远程 syslog 服务器

```text
switch (config) # logging x.x.x.x
```

### 设置 NTP 服务器

```text
switch (config) # ntp server x.x.x.x
```

## Cisco

### 设置 NTP 服务器

```text
# ntp server x.x.x.x
```

### 配置 Trunk

```text
# config terminal
(config)# interface ethernet 1/1
(config-if)# switchport mode trunk
(config-if)# switchport trunk allowed vlan 12-34
```

### 配置 Access

```text
# config terminal
(config)# interface ethernet 1/1
(config-if)# switchport mode access
(config-if)# switchport access vlan 1234
```
