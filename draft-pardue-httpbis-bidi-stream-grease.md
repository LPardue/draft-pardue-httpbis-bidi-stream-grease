---
title: Greasing Bidirectional HTTP Streams
abbrev: HTTP Bidi Grease
docname: draft-pardue-httpbis-bidi-stream-grease-latest
category: std

ipr: trust200902
area: Applications and Real-Time
workgroup: HTTP
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, docindent, sortrefs, symrefs, strict, compact, comments, inline]

author:
    ins: L. Pardue
    name: Lucas Pardue
    org: Cloudflare
    email: lucaspardue.24.7@gmail.com

normative:

--- abstract

The behaviour of HTTP/2 and HTTP/3 bidirectional streams can be modified by
protocol extensions. Extensions of this type require negotiation and it is
possible that, for lack of driving application use case and/or deployment, this
form of extension is not tested widely. This memo defines a mechanism for
endpoints to exercise the extension point at the stream and framing layer.

--- note_Note_to_Readers

*RFC EDITOR: please remove this section before publication*

Source code and issues list for this draft can be found at
<https://github.com/LPardue/draft-pardue-http-bidi-stream-grease>.

--- middle

# Introduction

The behaviour of HTTP/2 and HTTP/3 bidirectional streams can be modified by
protocol extensions. Extensions of this type require negotiation and it is
possible that, for lack of driving application use case and/or deployment, this
form of extension is not tested widely. This memo defines a mechanism for
endpoints to exercise the extension point at the stream and framing layer.

In HTTP/2 {{!HTTP2=RFC7540}} all streams are bidirectional. Client-initiated
streams are conventionally used for requests, while server-initiated streams are
used for push responses. HTTP message exchanges occur on streams; see Section 8
of {{HTTP2}}. Streams have several states. Depending on the stream initiator and
the endpoint role, sending or receiving particular frame types or frames with
flags (END_STREAM, RST_STREAM) causes transition between states; see Section 5.1
of {{HTTP2}}. Extensions are permitted to modify these behaviours but
negotiation is required; see Section 5.5 of {{HTTP2}}.

In HTTP/3 {{!HTTP3=I-D.ietf-quic-http}} client-initiated bidirectional streams
are conventionally used for requests, while server-initiated streams are not
used. HTTP message exchanges occur on streams; see Section 4.1 of {{HTTP3}}. Stream states are managed by the QUIC transport layer. Extensions are permitted to modify message exchange behaviour but negotiation is required; see Section 9 of {{HTTP3}}.

Although HTTP/2 and HTTP/3 extensions that change bidirectional stream behaviour
are possible, there are not many cases of such extensions and not much evidence
of interoperable deployments. This document defines bidirectional stream grease mechanisms for HTTP/2 and HTTP/3 that are intended to exercise the extension point without requiring any functional application changes.


## Notational Conventions

{::boilerplate bcp14}

# HTTP/2 Bidirectional Stream Extension
The HTTP/3 bidirectional stream grease extension is an optional change to
client- and server- initiated bidirectional streams. It is negotiated via the
SETTINGS_BIDI_GREASE setting (0xTBD). A value of 0 indicates the endpoint does
not support receiving bidirectional grease streams, a value of 1 indicates that
an endpoint does support receiving bidirectional stream grease. The default is
0. An endpoint that receives SETTINGS_BIDI_GREASE with a value that is neither 0 or 1 MUST terminate the connection with error PROTOCOL_ERROR.

Once an endpoint receives SETTINGS_BIDI_GREASE with a value of 1, it MAY send a
BIDI_GREASE frame as the first element on a bidirectional stream. The
BIDI_GREASE frame immediately opens and terminates a stream. This is a change to
the stream state machine defined in Section 5.1 of {{HTTP2}}.

~~~~~~~~~~ drawing
+---------------------------------------------------------------+
|                           Data (*)                            |
+---------------------------------------------------------------+
~~~~~~~~~~
{: #fig-h2 title="HTTP/2 BIDI_GREASE frame"}

The BIDI_GREASE frame does not define any flags.

The data carried in a BIDI_GREASE frame has no semantic meaning. It MUST be
discarded.

BIDI_GREASE frames MUST be associated with a stream. If a BIDI_GREASE frame is
received with a stream identifier of 0x0, the recipient MUST treat this as a
connection error of type PROTOCOL_ERROR.


# HTTP/3 Bidirectional Stream Grease

The HTTP/3 bidirectional stream grease extension is an optional change to
client- and server- initiated bidirectional streams. It is negotiated via the
SETTINGS_BIDI_GREASE setting (0xTBD). A value of 0 indicates the endpoint does
not support receiving bidirectional grease streams, a value of 1 indicates that
an endpoint does support receiving bidirectional stream grease. The default is
0. An endpoint that receives SETTINGS_BIDI_GREASE with a value that is neither 0 or 1 MUST terminate the connection with error H3_SETTINGS_ERROR.

Once an endpoint receives SETTINGS_BIDI_GREASE with a value of 1, it MAY send a
BIDI_GREASE frame as the first element on a bidirectional stream. BIDI_GREASE is
a type-value frame, which means its data extends to the full length of the
stream.

~~~~~~~~~~ drawing
BIDI_GREASE {
    Type = (0xTBD),
    Data = (..)
}
~~~~~~~~~~
{: #fig-h3 title="HTTP/3 BIDI_GREASE frame"}

The data carried in a BIDI_GREASE frame has no semantic meaning. It MUST be
discarded.

BIDI_GREASE frames MUST NOT be sent on the control stream or push streams. If a
frame is received on one of these stream types, it MUST be treated as a
connection error of type H3_FRAME_UNEXPECTED.

# Security Considerations

Bidirectional stream grease extensions allow arbitrary data to be sent. The
protocol prohibits endpoints from unintentionally processing such data as HTTP
messages.

# IANA Considerations

TBD

--- back

# Acknowledgements

The ideas in this document were discussed during a WebTransport Working Group
meeting.

