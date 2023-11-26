---
title: "HTTP/3 over TCP"
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
  RFC9110:

--- abstract

This document specifies how to use HTTP/3 on top of TCP.


--- middle

# Introduction

In the year of 2023, HTTP/2 ({{?RFC9113}}) is the most widely used version of
HTTP across the Internet, while the adoption rate of HTTP/3 ({{!RFC9114}}) is
increasing rapidly.

HTTP/3 has several benefits over HTTP/2 thanks to the use of QUIC ({{?RFC9000}})
as the transport layer protocol that provides features such as stream
multiplexing without head-of-line blocking, low-latency connection
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
of TCP, by using Quic Services for Streams
({{!I-D.kazuho-quic-quic-services-for-streams}}) as the basis. QUIC Services
for Streams is a polyfill of QUIC that runs on top of bi-directional streams. By
using QUIC Services for Streams, it is possible to run HTTP/3 on top of TCP
without modification.

Design, implementation, and maintenance can focus on just one HTTP version:
HTTP/3.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Starting HTTP/3 over TCP

HTTP/3 over TCP can be used for “http” and “https” URI schemes defined in
{{Section 4.2 of RFC9110}} with the same default port numbers as HTTP/1.1
({{!RFC9112}}).

When starting HTTP/3 for “https” URIs on top of TCP, clients use the TLS
({{!RFC8446}}) with the ALPN ({{!RFC7301}}) extension: “h3t”.

Also, clients may learn that a particular server supports HTTP/3 over TCP by
other means. A client that knows that a server supports HTTP/3 over TCP can
establish a TCP connection and start exchanging HTTP/3 frames using QUIC
Services for Streams.

The latter is the only way to discover HTTP/3 over TCP for “http” URIs.


# Exchanging HTTP/3 Frames

Once the TCP connection is setup and the Transport Parameters are exchanged by
QUIC Services for Streams, endpoints can exchange the HTTP/3 frames as if the
underlying connection was QUIC.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
