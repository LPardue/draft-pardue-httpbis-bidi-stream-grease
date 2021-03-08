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
protocol extensions. This memo defines a mechanism for endpoints to exercise the
extension point at the stream and framing layer.

--- note_Note_to_Readers

*RFC EDITOR: please remove this section before publication*

Source code and issues list for this draft can be found at
<https://github.com/LPardue/draft-pardue-http-bidi-stream-grease>.

--- middle

# Introduction

The behaviour of HTTP/2 and HTTP/3 bidirectional streams can be modified by
protocol extensions. This memo defines a mechanism for endpoints to exercise the
extension point at the stream and framing layer.

## Notational Conventions

{::boilerplate bcp14}



# Security Considerations



# IANA Considerations

TBD

--- back

# Acknowledgements

The ideas in this document were discussed during a WebTransport Working Group
meeting.

