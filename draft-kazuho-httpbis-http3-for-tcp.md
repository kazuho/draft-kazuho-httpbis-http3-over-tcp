---
title: "HTTP/3 for TCP"
docname: draft-kazuho-httpbis-http3-for-tcp-latest
category: std
ipr: trust200902
keyword: internet-draft
stand_alone: yes
pi: [toc, sortrefs, symrefs]
author:
 -
    fullname: Kazuho Oku
    organization: Fastly
    email: kazuhooku@gmail.com

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

This document resolves the problem by specifying a way to run HTTP/3 on top of
TCP, by using Quic Services for Streams
{{!QSS=I-D.kazuho-quic-quic-services-for-streams}} as the basis. QUIC Services
for Streams is a polyfill of QUIC that runs on top of bi-directional streams. By
using QUIC Services for Streams, it is possible to run HTTP/3 on top of TCP
without modification.

Design, implementation, and maintenance can focus on just one HTTP version:
HTTP/3.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
