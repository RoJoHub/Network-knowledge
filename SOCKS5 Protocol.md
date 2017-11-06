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
>
>使用网络防火墙，有效地将组织内部网络结构与外部网络（如因特网）隔离开来的系统正变得越来越流行。这些防火墙系统通常充当网络之间的应用层网关，通常提供受控制的telnet、FTP和SMTP访问。随着为促进全球信息发现而设计的更为复杂的应用层协议的出现，需要为这些协议提供透明和安全地穿越防火墙的总体框架。
>
>There exists, also, a need for strong authentication of such traversal in as fine-grained a manner as is practical.This requirement stems from the realization that client-server relationships emerge between the networks of various organizations, and that such relationships need to be controlled and often strongly authenticated.
>
>此外，还需要像实际那样以细粒度的方式对此类遍历进行强身份验证。这一要求源于在各种组织的网络之间出现客户机-服务器关系，并且需要控制这些关系，并且经常进行强身份验证。

 2. 现有的实践

>There currently exists a protocol, SOCKS Version 4, that provides for unsecured firewall traversal for TCP-based client-server applications, including TELNET, FTP and the popular information- discovery protocols such as HTTP, WAIS and GOPHER.
>
>目前存在着一个协议，SOCKS4的版本，提供了不安全的防火墙穿越的基于TCP的客户端-服务器应用程序，包括Telnet、FTP和流行的信息发现协议如HTTP，WAIS和GOPHER。
>
>This new protocol extends the SOCKS Version 4 model to include UDP, and extends the framework to include provisions for generalized strong authentication schemes, and extends the addressing scheme to encompass domain-name and V6 IP addresses.
>
>这个新协议扩展了SOCKS4的模型，包括UDP，并扩展了框架，以包含用于广义强认证方案的条款，并扩展了寻址方案以包含域名和V6 IP地址。
>
>The implementation of the SOCKS protocol typically involves the recompilation or relinking of TCP-based client applications to use the appropriate encapsulation routines in the SOCKS library.
>
>SOCKS协议的实施通常包括TCP重新编译或重新连接的客户端应用程序在SOCKS library中使用适当的封装例程。
>
>注：
>Unless otherwise noted, the decimal numbers appearing in packet- format diagrams represent the length of the corresponding field, in octets.  Where a given octet must take on a specific value, the syntax X'hh' is used to denote the value of the single octet in that field. When the word 'Variable' is used, it indicates that the corresponding field has a variable length defined either by an associated (one or two octet) length field, or by a data type field.
>
>除非另有说明，出现的十进制数数据包格式图代表相应字段的字节长度。在一个给定的字节必须在一个特定的值，语法x'hh”是用来表示在这一领域的单字节值。当‘Variable’被使用，表明相应的区域有一个可变长度的定义由一个相关的（一个或两个字节）长度字段或数据类型的字段。

 3. 基于TCP的客户端的过程

>When a TCP-based client wishes to establish a connection to an object that is reachable only via a firewall (such determination is left up to the implementation), it must open a TCP connection to the appropriate SOCKS port on the SOCKS server system.The SOCKS service is conventionally located on TCP port 1080.If the connection request succeeds, the client enters a negotiation for the authentication method to be used, authenticates with the chosen method, then sends a relay request.The SOCKS server evaluates the request, and either establishes the appropriate connection or denies it.
>
>当基于TCP的客户端希望建立与只能通过防火墙可达的对象的连接（这种确定取决于实现）时，它必须打开到SOCKS服务器系统上相应的SOCKS端口的TCP连接。SOCKS服务通常位于TCP端口1080上。如果连接请求成功，客户端将进入协商状态.如果连接请求成功，客户端输入一个要使用的身份验证方法进行谈判，与所选择的方法，然后发送一个延迟的请求。SOCKS服务器评估请求，要么建立适当的连接，要么拒绝它。
>
>Unless otherwise noted, the decimal numbers appearing in packet-format diagrams represent the length of the corresponding field, in octets.  Where a given octet must take on a specific value, the syntax X'hh' is used to denote the value of the single octet in that field. When the word 'Variable' is used, it indicates that the corresponding field has a variable length defined either by an associated (one or two octet) length field, or by a data type field.
>
>除非另有说明，出现的十进制数数据包格式图代表相应字段的字节长度。在一个给定的字节必须在一个特定的值，语法x'hh”是用来表示在这一领域的单字节值。当‘Variable’被使用，表明相应的区域有一个可变长度的定义由一个相关的（一个或两个字节）长度字段或数据类型的字段。
>
>The client connects to the server, and sends a version identifier/method selection message:
>
>客户机连接到服务器，并发送版本标识符/方法选择消息：

                   +----+----------+----------+
                   |VER | NMETHODS | METHODS  |
                   +----+----------+----------+
                   | 1  |    1     | 1 to 255 |
                   +----+----------+----------+

>The VER field is set to X'05' for this version of the protocol.  The NMETHODS field contains the number of method identifier octets that appear in the METHODS field.
>
>VER字段是协议的版本号:05。NMETHODS字段以8进制字节表示的METHODS的方法数.
>
>The server selects from one of the methods given in METHODS, and sends a METHOD selection message:
>
>服务器从给出的方法中选择一个，并发送一个方法选择消息：

                         +----+--------+
                         |VER | METHOD |
                         +----+--------+
                         | 1  |   1    |
                         +----+--------+

>If the selected METHOD is X'FF', none of the methods listed by the client are acceptable, and the client MUST close the connection.The values currently defined for METHOD are:
>
> - '00' NO AUTHENTICATION REQUIRED
> - '01' GSSAPI
> - '02' USERNAME/PASSWORD
> - '03' to '7F' IANA ASSIGNED
> - '80' to 'FE' RESERVED FOR PRIVATE METHODS
> - 'FF' NO ACCEPTABLE METHODS
>
>如果选择的METHOD是'ff'，客户机列出的任何方法都不可接受，并且客户端必须关闭连接。以下为值所定义的方法：
>
> - '00' 不需要认证
> - '01' GSSAPI (通用安全服务应用程序接口)
> - '02' USERNAME/PASSWORD
> - '03' to '7F' IANA (Internet Assigned Numbers Authority-互联网号码分配局) ASSIGNED
> - '80' to 'FE' 保留私有的方法
> - 'FF' NO ACCEPTABLE METHODS
>
>The client and server then enter a method-specific sub-negotiation.Descriptions of the method-dependent sub-negotiations appear in separate memos.
>
>然后客户端和服务器进入到一个特定方法的协商过程。这个协商过程出现在单独的备忘录中。
>
>Developers of new METHOD support for this protocol should contact IANA for a METHOD number.  The ASSIGNED NUMBERS document should be referred to for a current list of METHOD numbers and their corresponding protocols.
>
>开发者想开发新的METHOD支持该协议,需要联系IANA(Internet Assigned Numbers Authority)以得到METHOD的号码。对于当前的方法号及其相应的协议清单，应参考指定的编号文件
>
>Compliant implementations MUST support GSSAPI and SHOULD support USERNAME/PASSWORD authentication methods.
>
>兼容的实现必须支持GSSAPI，应该支持用户名/密码认证方式。

 4. 请求(Requests)

 >Once the method-dependent subnegotiation has completed, the client sends the request details.  If the negotiated method includes encapsulation for purposes of integrity checking and/or confidentiality, these requests MUST be encapsulated in the method- dependent encapsulation.
>The SOCKS request is formed as follows:
>
>        +----+-----+-------+------+----------+----------+
>        |VER | CMD |  RSV  | ATYP | DST.ADDR | DST.PORT |
>        +----+-----+-------+------+----------+----------+
>        | 1  |  1  | X'00' |  1   | Variable |    2     |
>        +----+-----+-------+------+----------+----------+
>
>     Where:
>
> - VER    protocol version: X'05'
> - CMD
>   - CONNECT X'01'
>   - BIND X'02'
>   - UDP ASSOCIATE X'03'
> - RSV    RESERVED
> - ATYP   address type of following address
>   - IP V4 address: X'01'
>   - DOMAINNAME: X'03'
>   - IP V6 address: X'04'
> - DST.ADDR       desired destination address
> - DST.PORT desired destination port in network octet order
>
>The SOCKS server will typically evaluate the request based on source and destination addresses, and return one or more reply messages, as appropriate for the request type.
>
>一旦方法相关的子协商已经完成，客户端发送请求的细节。如果协商的方法包含了完整性检查(和/或)机密性的封装，这些请求必须封装在与方法相关的封装中。
>SOCKS请求形成如下：
>
>        +----+-----+-------+------+----------+----------+
>        |VER | CMD |  RSV  | ATYP | DST.ADDR | DST.PORT |
>        +----+-----+-------+------+----------+----------+
>        | 1  |  1  | X'00' |  1   | Variable |    2     |
>        +----+-----+-------+------+----------+----------+
>
>
> - VER    协议版本号:'05'
> - CMD
>   - CONNECT '01'
>   - BIND  '02'
>   - UDP ASSOCIATE '03'
> - RSV    RESERVED(保留)
> - ATYP   address type of following address(以下地址的地址类型)
>   - IP V4 address(IPV4): '01'
>   - DOMAINNAME(域名): X'03'
>   - IP V6 address(IPV4): X'04'
> - DST.ADDR       目标地址
> - DST.PORT desired destination port in network octet order(网络八位字节顺序中所需的目标端口。)
>
>SOCKS服务器通常会根据源地址和目标地址评估请求，并根据请求类型返回一个或多个回复消息。

 5. Addressing

>In an address field (DST.ADDR, BND.ADDR), the ATYP field specifies the type of address contained within the field:
>
> - '01'
>
>the address is a version-4 IP address, with a length of 4 octets
>
> - '03'
>
>the address field contains a fully-qualified domain name.  The first octet of the address field contains the number of octets of name that follow, there is no terminating NUL octet.
>
> - '04'
>
>the address is a version-6 IP address, with a length of 16 octets.
>
>在地址字段（DST.ADDR，BND.ADDR）中，ATYP字段指定字段中包含的地址的类型:
>
> - '01'
>
>该地址是版本4 IP地址，长度为4个八位字节
>
> - '03'
> 
>地址字段包含一个完全合格的域名。 地址字段的第一个八位字节包含后面名称的八位字节数，没有终止空字节。
>
> - '04'
>
>该地址是版本6 IP地址，长度为16个八位字节。

 6. Replies

>The SOCKS request information is sent by the client as soon as it has established a connection to the SOCKS server, and completed the authentication negotiations.  The server evaluates the request, and returns a reply formed as follows:
>
>        +----+-----+-------+------+----------+----------+
>       |VER | REP |  RSV  | ATYP | BND.ADDR | BND.PORT |
>        +----+-----+-------+------+----------+----------+
>        | 1  |  1  | X'00' |  1   | Variable |    2     |
>        +----+-----+-------+------+----------+----------+
>
>     Where:
>
> - VER    protocol version: X'05'
> - REP    Reply field:
>   - X'00' succeeded
>   - X'01' general SOCKS server failure
>   - X'02' connection not allowed by ruleset
>   - X'03' Network unreachable
>   - X'04' Host unreachable
>   - X'05' Connection refused
>   - X'06' TTL expired
>   - X'07' Command not supported
>   - X'08' Address type not supported
>   - X'09' to X'FF' unassigned
> - RSV    RESERVED
> - ATYP   address type of following address
>   - IP V4 address: X'01'
>   - DOMAINNAME: X'03'
>   - IP V6 address: X'04'
> - BND.ADDR       server bound address
> - BND.PORT       server bound port in network octet order
>
>Fields marked RESERVED (RSV) must be set to X'00'.
>
>If the chosen method includes encapsulation for purposes of authentication, integrity and/or confidentiality, the replies are encapsulated in the method-dependent encapsulation.
>
>SOCKS请求信息一旦与SOCKS服务器建立连接，就由客户端发送，并完成认证协商。 服务器评估请求，并返回一个形成如下的答复：
>
>        +----+-----+-------+------+----------+----------+
>       |VER | REP |  RSV  | ATYP | BND.ADDR | BND.PORT |
>        +----+-----+-------+------+----------+----------+
>        | 1  |  1  | X'00' |  1   | Variable |    2     |
>        +----+-----+-------+------+----------+----------+
>
>     Where:
>
> - VER    协议的版本号: '05'
> - REP    回复字段:
>   - '00' 成功
>   - '01' general SOCKS server failure
>   - '02' connection not allowed by ruleset
>   - '03' 网络无法访问
>   - '04' Host unreachable(主机无法访问)
>   - '05' Connection refused
>   - '06' TTL expired
>   - '07' Command not supported
>   - '08' Address type not supported
>   - '09' to 'FF' unassigned  (未定义)
> - RSV    保留字段
> - ATYP   以下地址的地址类型
>   - IP V4 address: '01'
>   - DOMAINNAME: '03'
>   - IP V6 address: '04'
> - BND.ADDR       server bound address(服务器绑定地址)
> - BND.PORT       server bound port in network octet order(服务器绑定的端口以网络八位字节顺序)
>
>标有RESERVED（RSV）的域必须设置为'00'。
>
>如果所选择的方法包括用于认证，完整性(和/或)机密性的封装，则应答被封装在依赖于方法的封装中。