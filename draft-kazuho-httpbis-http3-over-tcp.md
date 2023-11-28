---
title: "HTTP/3 on Streams"
docname: draft-kazuho-httpbis-http3-over-tcp-latest
category: std
wg: httpbis
ipr: trust200902
keyword: internet-draft
stand_alone: yes
pi: [toc, sortrefs, symrefs]
author:
 -
    fullname: Kazuho Oku
    organization: Fastly
    email: kazuhooku@gmail.com
normative:
  HTTP-SEMANTICS: RFC9110

--- abstract

This document specifies how to use HTTP/3 on top of bi-directional,
byte-oriented streams such as TLS over TCP.


--- middle

# Introduction

In the year of 2023, HTTP/2 {{?HTTP2=RFC9113}} is the most widely used version
of HTTP across the Internet, while the adoption rate of HTTP/3
{{!HTTP3=RFC9114}} is increasing rapidly.

HTTP/3 has several benefits over HTTP/2 thanks to the use of QUIC
{{?QUIC=RFC9000}} as the transport layer protocol that provides features such as
stream multiplexing without head-of-line blocking, low-latency connection
establishment.

But because QUIC is not as universally available or accessible as TCP, it is
unlikely that we will see all HTTP/2 traffic migrate to the QUIC-based HTTP/3.

The fact means that HTTP deployments have to support two transport protocols and
the HTTP versions running on each of them for the foreseeable future.

This is a costly situation, as the two HTTP protocols are different in many
aspects, such as wire-encoding, flow control and stream multiplexing machinery,
and HTTP header compression. Extensions that work at the HTTP wire encoding
layer have to be designed and implemented for both HTTP/2 and HTTP/3. Both the
protocol stacks have to be maintained, fixing bugs, performance issues, and
vulnerabilities as necessary.

This specification resolves the problem by defining a way to run HTTP/3 on top
of TCP, by using Quic on Streams
({{!QS=I-D.kazuho-quic-quic-services-for-streams}}) as the basis. QUIC on
Streams is a polyfill of QUIC that runs on top of bi-directional streams. By
using QUIC on Streams, it is possible to run HTTP/3 on top of TCP without
modification.

Design, implementation, and maintenance can focus on just one HTTP version:
HTTP/3.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Starting HTTP/3 on Streams

HTTP/3 on Streams can be used for “http” and “https” URI schemes defined in
{{Section 4.2 of HTTP-SEMANTICS}} with the same default port numbers as HTTP/1.1
{{!HTTP1=RFC9112}}.

When starting HTTP/3 for “https” URIs on top of TCP, clients use the TLS
{{!TLS13=RFC8446}} with the ALPN {{!ALPN=RFC7301}} extension: “h3t”.

Also, clients may learn that a particular server supports HTTP/3 on Streams by
other means. A client that knows that a server supports HTTP/3 on Streams can
establish a TCP connection and start exchanging HTTP/3 frames using QUIC on
Streams.

The latter is the only way to discover HTTP/3 on Streams for “http” URIs.

When used in cleartext, servers can determine if or not the client is speaking
HTTP/3 on Streams by comparing the first eight bytes to the encoded form of the
QS_TRANSPORT_PARAMETERS frame type (Section 4.2 of {{QS}}).


# Exchanging HTTP/3 Frames

Once the TCP connection is setup and the Transport Parameters are exchanged by
QUIC on Streams, endpoints can exchange the HTTP/3 frames as if the underlying
connection was QUIC.


# Support for Extended CONNECT

Servers speaking HTTP/3 on Streams MUST implement the Extended CONNECT scheme
defined in {{!EXT-CONNECT3=RFC9220}}.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
