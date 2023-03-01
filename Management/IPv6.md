# IPv6 :six:

`IPv6` is the most recent **Internet Protocol**.

## :pager: Basics

The `IPv6` `address` is **128 bits** (i.e. 16 bytes) or **8 groups of 2 bytes** (**hextet**) in hexadecimal numbers separated by colons:
```
2000:ABCD:ABCD:0000:0000:0000:0000:0001
```
Leading zeros of each block can be omitted, the above `address` can hence be written like this:
```
2000:ABCD:ABCD:0:0:0:0:1
```
We can abbreviate whole blocks of zeros with `::` and write:
```
2000:ABCD:ABCD::1
```
This can only be done *once* in order to void ambiguity:

| IP                | Statement          |
| ----------------- | -------------------|
|FE80:0:0:0:1:0:0:1 | Correct            |
|FE80::1:0:0:1      | Correct            |
|FE80:0:0:0:1::1    | Correct            |
|FE80::1::1         | Ambiguous, Wrong   |

## :telescope: Scopes

In `IPv4` the `router` has a unique `IP` in the whole `internet` and `NAT` is performed to reach
`hosts` behind the `router`.

With `IPv6` there is no `NAT` involved, a `host` can be reached from the `internet`
or not depending on what kind of `SCOPE` the `IP` belongs to.

Depending on how the `IP` looks, it belongs to a specific `SCOPE`. 

| Range     | Purpose            |Scope Name    | Can be reached from |
| --------- | -------------------|--------------|--------------|
| ::1/128   | Loopback address   |              | LocalHost    | 
| 2000::/3  | GLOBAL unicast     | GLOBAL       | Internet     |
| FC00::/7  | Unique-local       | UNIQUE LOCAL | LAN          |
| FE80::/10 | Link-Local Unicast | LINK LOCAL   | Same Switch  |


| Scope        | Example                                   |
| ------------ | ----------------------------------------- |
| GLOBAL       | 2000:ABCD:ABCD:0000:0000:0000:0000:0001   |
| UNIQUE LOCAL | FC00:ABCD:ABCD:0000:0000:0000:0000:0001   |
| LINK LOCAL   | FE80:ABCD:ABCD:0000:0000:0000:0000:0001   |


In `IPv4` the **Network Interface** only has one `IP` on the `LAN`.

In `IPv6` a **Network Interface** can have **multiple** `addresses` in different `scopes`.

Under this circumstance, a `host` can be reached from anywhere.

## :id: Subnetting

An `IPv6` IP is divided into 3 sections: `Network address`, `Subnet`, and `Device Id`.

The first 3 `hextets` are used for the **network address**, the 4th `hextet` is used for the **subnet Id** and the 
last 4 `hextets` are used as a `device Id` or **Interface Id**. 

In most `router interfaces` the `IPv6 IP` of a `host` is displayed as the last 4 `hextets` of the full `IP` 
and under the name **Interface ID**.
```
Interface Id example
::0000:0000:0000:0001
```

 The last 4 `hextets` are derived from the device MAC address.


| First 48 bits                  | Next 16 bits                    |Last 64 bits |
| -------------------------------| --------------------------------|-------------------------------------------------|
| **Network** address (3 groups) | **Subnet** address (1 group)    | **Device** address (**Interface ID**, 4 groups) |
|  FDDD:F00D:CAFE                | 0000                            | 0000:0000:0000:0001                             |
|  FC00:0000:0000                | ABCD                            | ABCD:ABCD:ABCD:ABCD                             |
|  2000:0000:0000                | ABCD                            | ABCD:ABCD:ABCD:ABCD                             |


## :phone: **ISP** and Subnets

If you have `IPv6` enabled by the **ISP**, you will be provided with a `subnet`.

The first 4 `hextets` will define your whole `subnet`. 

Example:
```
Subnet
2000:0000:0000:ABCD::/57  

IPs in this range
2000:0000:0000:ABCD::1
2000:0000:0000:ABCD:0000:0000:0000:0001

2000:0000:0000:ABCD:ABCD:ABCD:ABCD:1 
```

Note that the **ISP** will provide you with a subnet of `IPs` of the type `GLOBAL Scope`.

This means that you can assign your `static IP` to your `hosts` on your `network`
in this `subnet` range.

To expose such a `host` to the `internet` there is no `NAT` involved because it is a `globally unique address`,
even though the `host` is in your private `LAN`. 

For `IPv6`, `routers` will have at most `Firewall` rules to prevent `ports` from being reachable. 

If you allow `ports` to be shared for a `host` on your `LAN `then that `host` will be reachable from the `internet`
without `NAT forwarding`.

There are clear advantages and disadvantages to this. 

## :thumbsup: Advantages
The advantage is that your `network` can host an enormous amount of computers.

For a `2000:0000:0000:ABCD::/57` subnet you can host `2361183241434822606848` computers if you would have
enough cables and `switches`.

Since there is no `NAT` involved, each `host` has its `global IP`, and therefore each `host`
has `control` of its `exposed ports`. 

This means that you can host multiple `Web Servers`, each listening to `port 80`, from a single `LAN`.

Since you control the `subnet`, the `IPs` are not dynamic and you do not need a `DynamicDNS resolver` for the `hosts`.

## :thumbsdown:Disadvantages 

The disadvantages are clear here, your hosts are directly exposed to the `internet` and you have to manage 
`firewall` rules for each `host` individually. 

Your `hosts` are vulnerable to `port scanning` and `flooding` since the `IP` does not change dynamically unless you do.

### :globe_with_meridians: IPv6 and DNS

`DNS` records for an `IPv6 IP` are called `AAAA` (quad A) records.

### :globe_with_meridians: IPv6 addresses in URIs/URLs, Browser

Add brackets around the `IPv6 address` in the `browser address bar`:
```
http://[2a00:1450:4001:82a::2004]
```
To specify a `port` just add it at the end after a semi-colon:
```
http://[2a00:1450:4001:82a::2004]:8080
```

## :wrench: :hammer: Linux Tools

The following tools can be used to investigate `IPv6`.

### Testing connectivity to a host:
```
ping -6 <IP>
```
```
ping6 <IP>
```

### Testing egress (to outside) conectivity:
```
ping6 ipv6.google.com
```

### Testing ingress (to inside) conectivity

:point_right::link:[online ipv6 ping with results](http://www.ipv6now.com.au/pingme.php)

### Testing other link-local addresses
Link-local addresses have a limited scope, which means they are only visible in a certain context. 

That's how it's possible to have a link-local address on every interface although technically, the addresses all belong in the same subnet.

```
ping6 -I <network interface name>  <link-local adress of other host on network>
```
Or
```
ping6 fe80::aff1:8891:5fd1:1f2d%enp1s0
```
Where the interface Id is appended at the end with a '%' prefix.

### Find a route to the host:
```
route -A inet6
ip -6 route show [dev <device>]
```

## Forwarding in Ubuntu
To enable `port forwarding` in **Ubuntu** run this `command` and `reboot` the `system`:
```
sysctl -w net.ipv6.conf.all.forwarding=1
```

## FritzBox 6660 IPv6 Forwarding
Fritzbox steps to `port share` for `IPv6`
1. Internet > Permit access 
	Add Device
	
	Find IPv6 of device and replace IPv6 Interface ID fields
	Check - Permit Independent Port sharing for this device
	Check Enable Ping6
	Check Open this device to the internet
	
2. Home Network > Network > Network Settings > IP Addresses > IPv6 Settings button
	Change DHCPv6 Server in the Home Network
	To -> Assign Dns, Prefix(IA_PD), and IPv6 Address (IA_NA)


## :books: Documentation

:point_right::link:[IPv6 overview](https://docs.oracle.com/cd/E19120-01/open.solaris/819-3000/6n58i72ka/index.html)

:point_right::link:[IPv6 Global Unicast Address](https://docs.oracle.com/cd/E19120-01/open.solaris/819-3000/ipv6-overview-130/index.html#:~:text=The%20global%20unicast%20address%20is,is%20a%20global%20unicast%20address.)

:point_right::link:[Test IPv6](https://test-ipv6.com/)

:point_right::link:[IPv6 subnet calculator](https://www.internex.at/de/toolbox/ipv6)

:point_right::link:[Online IPv6 ping with results](http://www.ipv6now.com.au/pingme.php)

:point_right::link:[Stateless Address Autoconfiguration SLAAC](https://www.networkacademy.io/ccna/ipv6/stateless-address-autoconfiguration-slaac#:~:text=SLAAC%20stands%20for%20Stateless%20Address,is%20assigned%20to%20which%20node.)








