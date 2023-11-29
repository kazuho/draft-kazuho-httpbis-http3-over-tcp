---
title: "HTTP/3 on Streams"
docname: draft-kazuho-httpbis-http3-on-streams-latest
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

As of 2023, HTTP/2 {{?HTTP2=RFC9113}} remains the most widely used version of
HTTP across the Internet, although the adoption rate of HTTP/3
{{!HTTP3=RFC9114}} is increasing rapidly.

HTTP/3 has several advantages over HTTP/2, primarily due to its use of QUIC
{{?QUIC=RFC9000}} as the transport layer protocol, which provides features like
stream multiplexing without head-of-line blocking and low-latency connection
establishment.

However, given that QUIC's availability and accessibility are not as universal
as TCP's, a complete migration of all HTTP/2 traffic to QUIC-based HTTP/3 seems
unlikely.

This situation necessitates HTTP deployments to support both transport protocols
and their respective HTTP versions for the foreseeable future.

Maintaining dual support is costly, as the two protocols differ in many aspects
such as wire-encoding, flow control and stream multiplexing machinery, and HTTP
header compression. Extensions operating at the HTTP wire encoding layer must
be developed and implemented for both HTTP/2 and HTTP/3, and both protocol
stacks require ongoing maintenance to address bugs, performance issues, and
vulnerabilities.

To address this redundancy, this specification defines the method of running
HTTP/3 over TCP, utilizing QUIC on Streams
{{!QUIC-ON-STREAMS=I-D.kazuho-quic-quic-on-streams}} as the basis. QUIC on
Streams, acting as a polyfill of QUIC atop bi-directional byte streams, enables
the operation of HTTP/3 over TCP without any modifications.

Consequently, design, implementation, and maintenance efforts can concentrate on
a single HTTP version: HTTP/3.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Starting HTTP/3 on Streams

HTTP/3 on Streams can be used for “http” and “https” URI schemes defined in
{{Section 4.2 of HTTP-SEMANTICS}} with the same default port numbers as HTTP/1.1
{{!HTTP1=RFC9112}}.

When starting HTTP/3 for “https” URIs on top of TCP, clients use the TLS
{{!TLS13=RFC8446}} with the ALPN {{!ALPN=RFC7301}} extension: “h3s”.

Also, clients may learn that a particular server supports HTTP/3 on Streams by
other means. A client that knows that a server supports HTTP/3 on Streams can
establish a TCP connection and start exchanging HTTP/3 frames using QUIC on
Streams.

The latter is the only way to discover HTTP/3 on Streams for “http” URIs.

When used in cleartext, servers can determine if or not the client is speaking
HTTP/3 on Streams by comparing the first eight bytes to the encoded form of the
QS_TRANSPORT_PARAMETERS frame type (Section 4.2 of {{QUIC-ON-STREAMS}}).


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
