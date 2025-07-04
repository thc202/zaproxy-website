---
# This page was generated from the add-on.
title: Local Servers/Proxies
type: userguide
weight: 4
---

# Local Servers/Proxies

## Local Servers/Proxies

This tab allows you to configure the addresses and ports on which ZAP accepts incoming connections.

### Main Proxy

By default ZAP will listen on one local address and port, and usually these should be the address and port that you must configure your browser to use as a proxy.

### Additional Servers/Proxies

You can add as many additional addresses and ports for ZAP to listen on as you like.  
This can be useful when testing mobile applications - you can proxy the mobile application via a Wi-Fi interface at the same time as using the backend site directly using a browser proxying through ZAP emulating a mobile user agent.

### Server/Proxy Properties

Both the Main Proxy and the additional servers/proxies allow to configure the following properties.

#### Address

The local address to bound to. All of the available addresses are automatically identified.

#### Port

The port on which the server/proxy will listen.

#### Security Protocols

Allows to choose the SSL/TLS versions enabled for incoming connections (for example, from browsers). At least one version must be enabled, versions unsupported by the JRE will be unselected and disabled.   
The option SSLv2Hello must be selected in conjunction with at least one SSL/TLS version.

#### ALPN

Allows to enable and select the application protocols negotiated during a TLS handshake (ALPN extension).   
**Note:** The protocol falls back to HTTP/1.1 if the client establishing the connection does not support ALPN or any of the selected protocols.

#### Mode

Allows to specify if the API should be exposed or if it should be allowed to proxy requests. For example, one can configure a server exposing just the API to external addresses (i.e. remote control) while having a proxy for local addresses only.

#### Behind NAT

Indicates that the server/proxy (ZAP) is behind NAT. When selected ZAP will attempt to determine the public IP address, to properly detect and handle requests with the public IP address (for example, to be served by the ZAP API). Alternatively you can manually add the IP address as an Alias (refer to Aliases section).


**Note:** This option is only supported when ZAP is running in an AWS EC2 instance.
ZAP will obtain the public IP address from
[AWS EC2
instance's metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html#working-with-ip-addresses).  

ZAP should be started with this option enabled if access to the API, through the public IP address, is required:
> zap.sh -daemon -port 8080 -host 0.0.0.0 -config network.localServers.mainProxy.behindNat=true

Also, the API needs to be configured to accept external IP addresses (i.e. the IP address from where ZAP is being accessed).

#### Remove Accept-Encoding Request Header

Allows the proxy to remove the "Accept-Encoding" request-header field, so no (unsupported) encoding transformations are done to the response.  
This option should be always enabled unless when testing the encoding transformations.  
The messages encoded with unsupported encodings will not be correctly scanned (either by passive or active scanners).  
Supported content encodings:

* `br`, on Linux x64, macOS (Intel and Apple Silicon), and Windows x64
* `deflate`
* `gzip`

#### Decode Response

Allows the proxy to automatically decode (i.e. gzip, deflate) the response. This option is needed for applications that ignore the "Accept-Encoding" request-header field.

### Intercepting/Transparent Proxy

The local proxies can be used as intercepting/transparent proxies for both HTTP and HTTPS. For HTTPS the client applications (e.g. browser) need to use the TLS extension [Server Name Indication](https://tools.ietf.org/html/rfc6066#section-3). This allows you to set up testing LANs (or VMs) where all HTTP and HTTPS traffic is proxied regardless of software settings.


For example, if you have a Linux machine you use for testing, you can do something like the following to forward all HTTP and
HTTPS traffic to a local proxy listening on `192.168.0.14:8080`:

    iptables -t nat -A OUTPUT -p tcp --dport 443 -j DNAT --to-destination 192.168.0.14:8080
    iptables -t nat -A OUTPUT -p tcp --dport 80 -j DNAT --to-destination 192.168.0.14:8080
    	
## Aliases

Allows to identify the local servers/proxies with other names (domain or address). For example, to access the ZAP API with a public IP address.

## Pass-through

Allows to configure the authorities (domain/address and port) that will pass-through ZAP, thus sending the data without being decrypted/encrypted. This is useful when a client has certificate pinning for certain hosts or the domain/data is irrelevant to the target being tested.

Requires that the client sends a CONNECT request with the required authority. The `authority` is interpreted as a regular expression and
needs to be present in the requested authority.

For example, to pass-through all connections to `example.org` it can be specified just `example.org`, therefore matching any port.

Using pass-through is more efficient than using excludes (e.g. Exclude from Proxy, Global Exclude URL) when all the traffic for the host is not relevant
to the tests.

## See also

|   |                                          |                                    |
|---|------------------------------------------|------------------------------------|
|   | [Network](/docs/desktop/addons/network/) | the introduction to Network add-on |
