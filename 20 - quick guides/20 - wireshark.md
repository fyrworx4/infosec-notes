# wireshark

## column customization

* Uncheck No, Protocol, Length
* Add Source Port and Destination Port
  * Source Port - src port (unresolved)
  * Destination Port - dest port (unresolved)
* Align Left the columns
* Change time to UTC
  * View > Time Display Format > Change to UTC Date and Time Of Day, change to Seconds
* Custom columns to show domains
  * Type in `http.request` in filter
  * Hypertext Transfer Protocol > Host
  * Apply host field to column
  * Type in `ssl.handshake.type == 1` in filter
  * Secure Sockets Layer > TLSv1.2 > Handshake > Extension: server_name > Server Name Indication > Server Name
  * Apply server name to column

## emotet

### view downloading initial binary

basic web filter

```
(http.request or tls.handshake.type eq 1) and !(ssdp)
```

basic web filter + responses

```
(http.request or http.response or tls.handshake.type eq 1) and !(ssdp)
```

Look at last web request, follow TCP traffic

Export object

### view c2 traffic

emotet traffic is sent using HTTP POST requests

```
http.request.method eq POST
```

two types

* first type of post request ends with HTTP/1.1
* other one ends with HTTP/1.1 (application/x-www-form-urlencoded)

### connection attempts

```
tcp.analysis.retransmission and tcp.flags eq 0x0002
```

