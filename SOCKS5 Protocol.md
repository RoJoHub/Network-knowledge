# SOCKS5 Protocol

## 本备忘录的状态

本文档指定了Internet标准跟踪协议。网络社区，并提出讨论和建议改进。请参考当前版本的“因特网”。

标准化状态的官方协议标准“（STD 1）以及该协议的状态。这份备忘录的分发是无限的。

## 致谢

这个备忘录描述了一个协议，它是前一个进化的过程。版本的协议，版本4 [1]。这个新协议源于积极讨论和原型实现。

    贡献者：

        Marcus Leech: Bell-Northern Research
        David Koblas: Independent Consultant
        Ying-Da Lee: NEC Systems Laboratory
        Lee: NEC Systems Laboratory
        LaMont Jones: Hewlett-Packard Company
        Ron Kuris: Unify Corporation, Matt
        Ganis: International Business Machines.

 1. 介绍

>The use of network firewalls, systems that effectively isolate an organizations internal network structure from an exterior network,such as the INTERNET is becoming increasingly popular.These firewall systems typically act as application-layer gateways between networks, usually offering controlled TELNET, FTP, and SMTP access.With the emergence of more sophisticated application layer protocols designed to facilitate global information discovery, there exists a need to provide a general framework for these protocols to transparently and securely traverse a firewall.

    使用网络防火墙，有效地将组织内部网络结构与外部网络（如因特网）隔离开来的系统正变得越来越流行。
    这些防火墙系统通常充当网络之间的应用层网关，通常提供受控制的telnet、FTP和SMTP访问。
    随着为促进全球信息发现而设计的更为复杂的应用层协议的出现，需要为这些协议提供透明和安全地穿越防火墙的总体框架。

>There exists, also, a need for strong authentication of such traversal in as fine-grained a manner as is practical.This requirement stems from the realization that client-server relationships emerge between the networks of various organizations, and that such relationships need to be controlled and often strongly authenticated.

    此外，还需要像实际那样以细粒度的方式对此类遍历进行强身份验证。
    这一要求源于在各种组织的网络之间出现客户机-服务器关系，并且需要控制这些关系，并且经常进行强身份验证。

 2. 现有的实践

>There currently exists a protocol, SOCKS Version 4, that provides for unsecured firewall traversal for TCP-based client-server applications, including TELNET, FTP and the popular information- discovery protocols such as HTTP, WAIS and GOPHER.

    目前存在着一个协议，SOCKS4的版本，提供了不安全的防火墙穿越的基于TCP的客户端-服务器应用程序，包括Telnet、FTP和流行的信息发现协议如HTTP，WAIS和GOPHER。

>This new protocol extends the SOCKS Version 4 model to include UDP, and extends the framework to include provisions for generalized strong authentication schemes, and extends the addressing scheme to encompass domain-name and V6 IP addresses.

    这个新协议扩展了SOCKS4的模型，包括UDP，并扩展了框架，以包含用于广义强认证方案的条款，并扩展了寻址方案以包含域名和V6 IP地址。

>The implementation of the SOCKS protocol typically involves the recompilation or relinking of TCP-based client applications to use the appropriate encapsulation routines in the SOCKS library.

    SOCKS协议的实施通常包括TCP重新编译或重新连接的客户端应用程序在SOCKS library中使用适当的封装例程。

  注：
>Unless otherwise noted, the decimal numbers appearing in packet- format diagrams represent the length of the corresponding field, in octets.  Where a given octet must take on a specific value, the syntax X'hh' is used to denote the value of the single octet in that field. When the word 'Variable' is used, it indicates that the corresponding field has a variable length defined either by an associated (one or two octet) length field, or by a data type field.

    除非另有说明，出现的十进制数包格式图代表相应字段的长度，以字节。
    在一个给定的字节必须在一个特定的值，语法x'hh”是用来表示在这一领域的单字节值。
    当‘Variable’单词被使用时，表明相应的区域有一个可变长度的定义由一个相关的（一个或两个字节）长度字段或数据类型的字段。

3. 基于TCP的客户端的过程

>When a TCP-based client wishes to establish a connection to an object that is reachable only via a firewall (such determination is left up to the implementation), it must open a TCP connection to the appropriate SOCKS port on the SOCKS server system.

    当基于TCP的客户端希望建立与只能通过防火墙可达的对象的连接（这种确定取决于实现）时，它必须打开到SOCKS服务器系统上相应的SOCKS端口的TCP连接。

>The SOCKS service is conventionally located on TCP port 1080

    SOCKS服务通常位于TCP端口1080上。如果连接请求成功，客户端将进入协商状态.

> If the connection request succeeds, the client enters a negotiation for the authentication method to be used, authenticates with the chosen method, then sends a relay request. 

    如果连接请求成功，客户端输入一个要使用的身份验证方法进行谈判，与所选择的方法，然后将继电器的要求。