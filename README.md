# http-review
**A COMPARATIVE REVIEW OF HTTP/1.1, HTTP/2 and HTTP/3**

The Hypertext Transfer Protocol (HTTP) was first adopted by the World-Wide Web global information initiative in 1990. The timeline across the versions has been largely sequential with each version aiming to fill the gaps in the previous one.
  - HTTP/0.x versions got the core mission up and running—a stateless application-level protocol for distributed, collaborative, hypertext information  systems. 
  - HTTP/1.x solved numerous details including the need for persistent connections and name-based virtual hosts. 
  This version became extremely successful and essentially established the modern world wide web. 
  - S-HTTP was an encryption-enabled enhancement of HTTP/1.x which later led to the encryption layer standard, Secure Sockets Layer(SSL). SSL in turn gave way to Transport Layer Security(TLS). 
  - HTTP/2, was born out of the need for efficiency in message parsing and latency reduction in version HTTP/1.x 
  This version introduced binary message framing, multiplexing and other extensions while leaving the core functioning of the HTTP protocol unchanged. 
  - HTTP/3, the latest version at this writing, similarly leaves the core functioning of HTTP unchanged, but enhances framing and multiplexing and security mechanisms to add reliability and reduce message transmission latency. 
  - This section is a comparative summary of the HTTP versions from 1.1 - 3.


<details><summary>Introduction</summary>
  <p>

Hypertext Transfer Protocol(HTTP) is “a stateless application-level protocol for distributed, collaborative, hypertext information  systems.” [3]. This protocol enables communication of HTML data and other web resources between different user agents and servers. Through the protocol, a message sender lets a receiver know the format of data representation so as they can be able to appropriately process the exchanged web resource. The exchange occurs between a server and a client. [1]

  HTTP was designed to be a generic interface for communication on the internet without regard to types of resources being exchanged or implementation of communicating HTTP clients. Due to this intended general applicability, the protocol specification is limited to defining syntax of communication, with corresponding meaning and expected behavior. It describes the architectural elements of the communication as well as all the mechanisms of network connection management and message handling.[3]

  Key terminologies in the HTTP specification include: connection which refers to the transport layer protocol between the client and the server. The client is the initiator of the connection while the server is the other side of that communication. The request is the HTTP initiating message while the response is the HTTP reply. The resource is any piece of data identifiable by HTTP’s Uniform Resource Identifier(URI) scheme. The scheme specifies the target resource by name, location, other properties and its relationship with other resources. The user agent is the client which can be a browser or any other end-user—facing application including command shells, mobile apps and even household appliances. [2] [3]

  The data exchange model used by HTTP is referred to as a client-server model. The client sends a request to a server and the server responds with the requested web resource, with the data format—HTTP version—specified in the header. The common scenario is that a web user either types a Uniform Resource Locator(URL) into the web browser search area or clicks on a URL link and the browser who is the client translates these URLs into HTTP requests and sends them to the server. The server then responds with the requested resource after accordingly fetching its components whether they are stored in one server or distributed in several locations. [1]

   User --url--> Client
     Client -----HTTP Request-------> Server
   Server <----->Data storage
           Client <----HTTP Response--------Server
   User <-parse & display--- Client

  The client then parses the HTTP response received which may include any web resources—such as text, images, videos, scripts. The browser also parses the formatting information present in the Cascading Style Sheets(CSS) specification for the received data, allowing it to present the information to the user in an organized format.[1]

  HTTP operates in the application layer while relying on a transport layer protocol for to build connections. Prior to HTTP/3, the transport protocol has been TCP, and has been used both in encrypted and unencrypted versions. However HTTP/3 is designed to run over UDP. 

  The communication between server and client is typically relayed between several virtual and physical components in between the two principals.[1] These intermediaries include proxies, tunnels and gateways[3]. Proxies may perform several functions including caching, authentication and content filtering[1]. Tunnels on the other hand are blind relays which do not change the message. They extend a virtual connection through a proxy. An example of tunnelling is when TLS channels confidential information through a firewall proxy[3]. 
  While tunnels relay blindly, other intermediaries are required to ensure that the protocol version in the first line of a HTTP message matches their own or otherwise revise it to match before forwarding. This allows for future messages from the receiver to be suitably encoded so that they can be correctly translated on their way back to the original sender.[3]

  HTTP version numbers have two parts: the major and the minor. The format of numbering is HTTP/major.minor. The major number indicates the protocol version which specifies the HTTP syntax of the message it accompanies and also implies what formats the sender can receive and process. The minor specifies the highest edition within that major version to which the sender conforms. This is useful for indicating the sender’s backwards compatibility within a version e.g. HTTP/1.0, HTTP/1.1. [2], [3]. 

  There have been numerous changes to HTTP since its initial version. The remainder of this writing reviews comparatively, three versions of HTTP—Versions 1.1 , 2 and 3. 
    We start off comparing the backgrounds, then the message architecture, connection establishment and management schemes, and other key properties.

  </p>
</details>

<details><summary> Overview of Version Evolution </summary>
<p>
The earliest versions of HTTP - version 0.9 - succeeded in transferring raw data across the internet but was not designed to organize such data. It was version 1.0 that organized the transferred data and added meta-data and semantics of request and response modifiers. Later version 1.1 introduced handling of hierarchical proxies, caching, connection persistence, virtual hosts and standardization of HTTP client implementation requirements to make it possible to correctly determine a client’s HTTP capability.[5]. 

HTTP/1.1 also introduced persistent connections as a default, allowing multiple request/response exchanges. It also introduced the transfer encoding header field to record any encodings applied or intended for a message during transfer[3] . This version also introduced multi-homed web servers with the requirement that clients and servers support the Host header field, report an error if the Host header field is missing from an HTTP/1.1 request and accept absolute URIs [3].

HTTP/2 was introduced mainly to improve the message transport mechanism to reduce latency, reduce protocol overhead by header compression, enable request prioritization and also support server push mechanism[ 6 ]. The protocol introduced binary framing of messages for transport, multiplexing, congestion control, flow control, parallelism, header compression and proactive response “push” by the server [ 7 ].

HTTP/2 is designed as an extension to its predecessor, so as to achieve its goals without altering the core functionality of HTTP/1.1. To avoid tampering with version 1.1, version 2 introduces a framing layer under the HTTP component but within the application layer for message formatting into frames, and for its other functions [ 7 ] . This version of HTTP is based on the SPDY protocol. Before adoption by HTTP, the SPDY protocol was designed to transport content at low latency on the web. In order to interoperate with HTTP, it was designed with two layers —the upper one to integrate with HTTP application servers, and the lower one for framing, multiplexing, compression prioritization [8].

HTTP/3  [16] is the latest version of this protocol. It was adapted from the QUIC protocol and has only recently—in 2018—been renamed to HTTP. This version builds upon HTTP/2 semantics. It improves parallelism by introducing per-stream multiplexing and flow control thereby introducing reliability to each stream separately. It also incorporates TLS 1.3 and reduces connection setup latency.[ 9 ].

The next section compares the features of the three versions HTTP/1.1, HTTP/2 and HTTP/3 in more detail.

</p>
</details>

<details><summary> Comparison Of Key Features</summary>
MESSAGE FRAMING 
<p>
  
HTTP/1.x is a plain text(ASCII) based protocol meaning its content is transferred in a human-readable format[10 ]. HTTP/1.x is therefore easy to read and interpret, which is a property suitable for early protocol development.  

In contrast, HTTP/2 is a binary protocol. Its messaging is intended to be read by a machine [11]. It was developed after HTTP/1 had become highly successful, to improve on transport latency. HTTP/2 does not tamper with the components of HTTP/1.x but rather changes the way the messages are framed for transmission[ 6 ]. 

It avoids tampering with HTTP/1 elements by introducing a framing layer that allows for the additional functions to be carried out without interfering with the HTTP elements above it or the transport layer(TCP) below it [12]. The binary formatting of HTTP/2 makes it more compact and less susceptible to error. This allows HTTP/2 messages to be parsed in one uniform method compared to its predecessor which needs multiple methods to parse a message due to extraneous elements [ 7] . 


+-----------------------------------------------+
 |                 Length (24)                   |
 +---------------+---------------+---------------+
 |   Type (8)    |   Flags (8)   |
 +-+-------------+---------------+-------------------------------+
 |R|                 Stream Identifier (31)                      |
 +=+=============================================================+
 |                   Frame Payload (0...)                      ...
 +---------------------------------------------------------------+

Figure 1: Frame Layout in HTTP/2 -  [ 7 ]. 


HTTP/3 is a transport optimization protocol. It builds on the HTTP/2 framing system and also adopts the multiplexing and flow control features of its predecessor. It adds congestion control, reliability, security—moves to TLS 1.3— as well as adds per-stream multiplexing so that messages on different streams do not interfere with each other. Due to this added reliability, it runs on UDP[ 12 ] which reduces connection setup latency and also message transfer latency. One might view the per stream multiplexing over UDP as having multiple small TCP-like connections. 

One key difference between the two versions is that while HTTP/2 provides an absolute ordering of frames spanning across all streams, QUIC  does the ordering per each stream separately. QUIC uses frames in the control stream, request streams and push streams. [ 13 ].

Each QUIC frame includes a length field for payload length, type field for the frame type and a payload segment. A frame not containing exactly the expected number of octets is treated as a connection error i.e. HTTP_MALFORMED_FRAME.
  0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                           Length (i)                        ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |    Type (8 octects)   |               Frame Payload (*)             ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Figure: HTTP/QUIC frame- [ 13 ]

</p></br>
  
HEADER FORMAT
  <p>
    Allows client and server to exchange additional information about resource involved in a connection or about the connection, or the participants. This information is contained within the header fields. In HTTP 1.1, each field in the header has a name and a value separated by a colon. Names are case insensitive. Fields may be spread over multiple lines, each additional line beginning with an SP(space) or HT. HTTP/1.x, headers are transferred in plain text format. [ 14 ]

Example formulation
     	 message-header = field-name ":" [ field-value ]
       field-name     = token
       field-value    = *( field-content | LWS )
       field-content  = <the OCTETs making up the field-value
                        and consisting of either *TEXT or combinations
                        of token, separators, and quoted-string>
Source: in RFC 2616 [ 14 ]

There are four types of header fields in HTTP/1.1: General-header, entity-header, request header, response-header. Each of these is extensible. 

General header contains fields that apply to both request and response but not the entity. Examples: general-header examples: Cache-Control|Connection | Date| Pragma| Trailer| Transfer-Encoding | Upgrade | Via|Warning     
  
Entity-header defines entity body and are only included when the message has a body. Examples: entity-header  examples: Allow   | Content-Encoding //data type
| Content-Length //length of a message |Content-Type//data type     

HTTP/2 maintains the same header field semantics from HTTP/1.1. The only change to the HTTP/1.1 header fields is that all field names are lower case and the request line  is split into separate pseudo-header fields :method, :scheme, :authority, and :path.[ 6 ]. All requests must include exactly one valid value for these fields. There is no definition for how to carry the version identifier included in HTTP/1.1.

In preparing headers for transfer in HTTP/2, header data redundancy is reduced by compression. This helps reduce request sizes allowing multiple requests to fit in a single packet.[7] .  Compression is used to serialize a header field list into a header block--cookie header field is treated differently. The block is then divided into a sequence of octets known as header block fragments. These fragments are transmitted within the payload of HEADER, PUSH_PROMISE or CONTINUATION frames.[ 7 ]. 

The fragments of one block must be transmitted as a contiguous sequence not intervened by other frames of any type. The receiver then reassembles the block by concatenating its fragments and then decompresses it to retrieve the field list. If the receiver is unable to perform this decompression it should terminate with a COMPRESSION_ERROR. [ 7 ]
SPDY protocol where HTTP/2 came from, proposed GZIP encoding for header compression but that was abandoned when an attack named CRIME was discovered, targeting GZIP compressed HTTP headers [ 7 ] . Consequently, a special compression mechanism was created for HTTP/2. This scheme is  known as HPACK. 

The HPACK compression format uses huffman encoding and requires both client and server maintain a list of all previously seen header fields to be used as a context reference for encoding previously transmitted or duplicate values. HPACK maintains a static table with a list of common HTTP header fields that all connections will likely use, and a dynamic table initially empty then updated based on values exchanged within the given connection. The effect of this is that each request is smaller with only values not seen before being encoded, and the rest represented by substitution values. [ 6 ].
Example: 

HTTP/3 adopts most of HTTP/2 header treatments e.g. converting field names to lowercase before encoding, and the special pseudo-header fields for target URI, method of request and response status. [ 9 ]

The compression encoding scheme changes in HTTP/3 to a variant of HPACK known as QPACK. This scheme allows for avoiding head-of-line blocking induced by header compression. Head-of-line blocking is unsuitable in QUIC due to lack of frame ordering across all streams[ 15 ]. Implementations of QUIC  can restrict maximum size of the header accepted per HTTP message, violation of which is treated as an error i.e. HTTP_EXCESSIVE_LOAD.  [ 9 ]. 

QPACK allows for correct out-of-order delivery. It preserves ordering of header fields within each header field list during encoding and in turn the encoded ordering is maintained by the decoder. This variant maintains the same reference tables as in HPACK but here the table entries are addressed separately. [ 15 ].


    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Header Block (*)                      ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

     Figure 5: HEADERS frame payload -  [ 17 ] 

   HEADERS frames are only sent on request streams or push streams.
    
  </p></br>
    
MESSAGE STRUCTURE
    <p>
    HTTP/1.1 Message format is as follows:
message   = start-line
                      *( header-field CRLF )
                      CRLF //text line break
                      [ message-body ]

Start line: If the message is a request, then the start line is a request line. If message is a response, then the start line is a status line.
start-line  = request-line / status-line
Request line:  first line of a request message. Format:
	request-line   = method SP request-target SP HTTP-version CRLF
Status line: first line of a response message. Format:
status-line = HTTP-version SP status-code SP reason-phrase CRLF
Header Fields: are extensible i.e. unlimited new field names can be introduced. They need be registered with the IANA registry. Their order of appearance in a message does not matter and intermediaries need not understand them to forward them if they are not specifically blocked key words. Line folding is not permitted within HTTP/1.1 header fields.
Message body carries the payload 
message-body = entity-body
                    | <entity-body encoded as per Transfer-Encoding>
Message body length - If transfer-encoding is present and its value is not identity, the length is defined by the “chunked” parameter. Each chunk contains a chunk-size value in the header. Else If the content-length header is present and entity length == transfer length then the value of length is recorded in this header. This field is only used in messages that do not contain the Transfer-Encoding header field. Else if length is not specified, the self-delimiting media used to transfer it is defines its length. If there is no message, then the first empty line after the header fields marks the end of the transfer and indicates that there is no message body.

HTTP/2 maintains the HTTP/1.1 message request and response semantics but provides an alternative syntax [ 7 ] . DATA frames contain arbitrary, variable-length sequences of octets associated with a stream. Padding may be added to the data frame as a security feature to hide the message size. Data frames also contain flags and error codes to indicate the status of the stream or the data. Data frames should only be sent if the connection is either open or half closed. They are subject flow control. [16].

   +---------------+
    |Pad Length? (8)|
    +---------------+-----------------------------------------------+
    |                            Data (*)                         ...
    +---------------------------------------------------------------+
    |                           Padding (*)                       ...
    +---------------------------------------------------------------+

                       Figure 6: DATA Frame Payload -  [16] 


HTTP/3 messages are sent in packets. After the packet protection is removed, the payload consisting of a sequence of frames is retrieved. Packets contain at least one frame and may have multiple frames and frame types. The exceptions are: Version Negotiation, Stateless Reset, and Retry packets which do not contain frames[ 17 ]. 

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                          Frame 1 (*)                        ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                          Frame 2 (*)                        ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
                                  ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                          Frame N (*)                        ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    Figure 8: QUIC Payload - [12]

Each data frame must be part of a specific request or response and must not be received on a control stream.

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                         Payload (*)                         ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    Figure 4: DATA frame payload - [ 12] 

Other Frame Types
PRIORITY frames can be used by a sender to advise on priority but must only be sent through the control stream. The GOAWAY frame is sent by a server to initiate a connection shutdown, after which it can stop accepting new requests and process only received ones. These frames can be used to make way for administrative actions like server maintenance. Clients do not send GOAWAY frames but rather they initiate a shutdown by stopping to make requests. Clients can send a MAX_PUSH_ID frame to limit the number of pushes from a server. To specify that unknown frame types be ignored, reserved frames can be used [12]
    
  </p></br>
      
SERVER PUSH  
    <p>
    HTTP/1.1 does not provide for server pushed responses. Introducing this provision was one of the goals of HTTP/2.

HTTP/2 allows for pre-emptive responses to be sent together with corresponding PUSH_PROMISEs(i.e. requests that correspond to the pushed response) to a client in association with a previous request received from the client.  This is used when the server knows the client will need those responses in order to fully process the response to their original request. [ 7 ] . They are sent on the same request stream used by the client for the associated client-initiated request. The receiving client must verify that pushed requests come from an authoritative server or a proxy configured for the corresponding request. Pushed requests are cacheable and are considered validated from origin server. Promised requests must be cacheable, safe and not containing a request body[ 7 ] . A client can request server push be disabled, negotiating for that separately at each hop. Unwanted promise streams should be reset by the client.

Similarly, HTTP3 also allows for server push via a push stream. A server can push a promised request header to a client via a PUSH_PROMISE frame. A push stream header is used:

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Stream Type (8)|                  Push ID (i)                ...
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                       Figure 2: Push Stream Header - [ 13 ]
    
  </p></br>
  
  
SAMPLE EXCHANGE UNDER EACH OF THE 3 PROTOCOL VERSIONS  
  
   <p>
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     Example of HTTP/1.1 Message Exchange from rfc[3]:
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
For a GET request (Section 4.3.1 of [RFC7231]) on the URI "http://www.example.com/hello.txt":

   Client request:
     GET /hello.txt HTTP/1.1
     User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
     Host: www.example.com
     Accept-Language: en, mi

   Server response:
     HTTP/1.1 200 OK
     Date: Mon, 27 Jul 2009 12:28:53 GMT
     Server: Apache
     Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
     ETag: "34aa387-d-1568eb00"
     Accept-Ranges: bytes
     Content-Length: 51
     Vary: Accept-Encoding
     Content-Type: text/plain

     Hello World! My payload includes a trailing CRLF.

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  Example of HTTP/2 Exchange from rfc7540: [16]
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
GET REQUEST,
GET /resource HTTP/1.1           HEADERS
     Host: example.org          ==>      + END_STREAM
     Accept: image/jpeg                     + END_HEADERS
                                                              :method = GET
                                         	         :scheme = https
                                          	         :path = /resource
                                          	         host = example.org
                                         	         accept = image/jpeg

POST REQUEST
     POST /resource HTTP/1.1          HEADERS
     Host: example.org          ==>     - END_STREAM
     Content-Type: image/jpeg           - END_HEADERS
     Content-Length: 123                    :method = POST
                                                          :path = /resource
     {binary data}                                 :scheme = https

                                       	     	    CONTINUATION
                                      		      + END_HEADERS
                                         	       	     content-type = image/jpeg
                                         	     	     host = example.org
                                           	     	      content-length = 123

                                     	    DATA
                                     	       + END_STREAM
                                    	      {binary data}

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
  Example of HTTP/3 Handshake: [ 12 ]
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Client                                     	             Server

   Initial[0]: CRYPTO[CH] ->

                                    Initial[0]: CRYPTO[SH] ACK[0]
                          Handshake[0]: CRYPTO[EE, CERT, CV, FIN]
                                    <- 1-RTT[0]: STREAM[1, "..."]

   Initial[1]: ACK[0]
   Handshake[0]: CRYPTO[FIN], ACK[0]
   1-RTT[0]: STREAM[0, "..."], ACK[0] ->

                              1-RTT[1]: STREAM[55, "..."], ACK[0]
                                          <- Handshake[1]: ACK[0]

         Example of 1-RTT Handshake
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

After each of these handshakes, packets can be exchanged.
    
  </p>  
</details>

<details><summary>Connection Establishment and Management</summary>
UNDERLYING PROTOCOLS
  <p>
HTTP/1.1 runs over TCP.  The "http" URI scheme indicates a default connection of TCP over IP at port 80. These defaults can be configured to allow use of a proxy through a different connection, protocol or port[3]. The “https” scheme is used for secure connections running over transport layer security at port 443.


HTTP/2 also runs on the application layer over TCP. The client initiates the TCP connection. The same URI schemes are used as in HTTP/1.1. The protocol has two identifiers: identifier string “h2” indicates the use of TLS while the string "h2c" identifies the protocol where HTTP/2 is run over cleartext TCP i.e. no TLS.[ 7 ] 

HTTP/3 runs over UDP. Endpoints exchange packets over UDP datagrams. Additionally, QUIC packets except Version Negotiation and Retry packets use authenticated encryption with additional data (AEAD)[ 17 ]. This means security is built in at packet level, making TLS redundant.
    </p></br>
  
CONNECTION, MANAGEMENT & PERSISTENCE 
    <p>     
HTTP 1.1 connection setup is determined by each client based on target resource, proxy configuration and connection establishment. The server’s response uses the same connection chain established by the client. 

The process involves: Identifying a target resource based on URI. Then the client checks if a connection establishment is necessary to fulfil the request e.g. the response is not in cache or in local storage. If necessary, the client will establish the inbound connection. Connections are established on transport- or session-layer protocols and each connection creates one transport link. 

There are no request identifiers in HTTP so the response receive order is used to associate responses to requests. If a client has multiple requests, it is required to maintain a list of outstanding requests in the order sent and associate each nest response to the next unfinalized request.

The via header field is used to indicate the presence of intermediaries with each getting its own value entry. A proxy may transform messages unless a no-transform cache-control directive is included. In that case if the message must be transformed then a warn-code (214) must be added to the header.

HTTP/1.1 must support persistent connections by default,  and use the "close" connection option to signal when a  connection will not persist after the current exchange. A recipient determines the connection status based on the protocol version  of the most recently received message or the connection header field if available.[3]

If a connection closes prematurely, a client can open a new one and automatically retransmit aborted requests. However, if an automatic retry fails, it should not be retried automatically.

When a sender wants to close the connection, they should use the “close connection” option in the connection header field. No further requests should be sent or received and processed on that connection [3]. If the server wants to close the connection, it should do so in stages i.e. first send the “close” connection option and then initiate the close after to give the client a chance to read the last response with the close option. The server waits for a corresponding close from the client before fully shutting down. The CONNECT method is used to turn an entire connection into a tunnel to a remote host.

In HTTP2 the client initiates the TCP connection. The CONNECT method is used to establish a tunnel-- TLS session -- over a single HTTP/2 stream to a remote host for use with HTTPS [ 7 ].

In HTTP/3 a client initiates a bidirectional QUIC stream and sends a request on it. The server responds on the same stream. The pseudo-method CONNECT is used as in HTTP/2 i.e. to establish a tunnel-- TLS session -- over a single HTTP/2 stream to a remote host for use with HTTPS [ 4 ]. 

A proxy can establish a TCP connection to the server identified in the ":authority" pseudo-  header field to mediate between a TCP server and a QUIC client, mapping between HTTP data and QUIC frames. Either server or client can close the TCP connection. Requests can be prioritized by marking a stream as dependent on another, making that other the parent and allowing it to receive priority on resources. [16]

The QUIC connection can be used for many requests and responses without closing it. To maintain a connection,  clients are expected to use QUIC PING frames to keep it    open. The connection closes if each endpoint declares an idle timeout. It can also be closed by either endpoint stopping usage of the connection and letting it close. This is typically what the client does. The server closes the connection by sending a GOAWAY message to initiate a graceful shutdown. If an application closes, that can also close the QUIC connection by transmitting QUIC APPLICATION_CLOSE frame which contains an error code to inform the peer of the application close. The QUIC transport layer can also indicate to the application layer that the connection is terminated e.g. due to any of the above reasons or to a transport - level error. [ 13 ]
</p></br>

MESSAGE CONCURRENCY & STREAMS
    <p>     
HTTP 1.1 has no request identifiers so response receive order is used to associate responses to requests. If a client has multiple requests, it is required to maintain a list of outstanding requests in the order sent and associate each next response to the next unfinalized request. [ 3 ].

A client with persistent connections may pipeline requests and process them in parallel but must send responses in the order received.[ 4 ]. A client is not limited on the maximum number of simultaneous connections in HTTP/1.1 but is encouraged to minimize them due to problems such as congestion.

HTTP/2 limits concurrently active streams in the SETTINGS_MAX_CONCURRENT_STREAMS parameter of the settings stream. Both  "open" and "half-closed" streams count toward the maximum. Streams in “reserved” state to not count. Each endpoint advertises its maximum.

HTTP 3 streams in QUIC can be created by sending data and either endpoint can create multiple concurrent streams and interleave them with each other. An arbitrary number of streams can be run concurrently. Each endpoint can use its maximum stream ID to limit concurrent streams for the given connection and include it in the settings sent to the peer. The peer stays under the specified limit unless it is incremented.[17]

</p></br>

STREAMS, MULTIPLEXING & FLOW CONTROL   
    <p>     
HTTP 1.1 does not provide multiplexing or flow control. The server allows the underlying protocol’s flow control mechanism to resolve any overloads.[ 3 ]

HTTP2 provides flow control to manage resource constraints to ensure that streams on the same connection do not interfere with each other. Flow control is also provided for individual streams.[7] . Flow control is connection specific, based on window size updates, is directional with the receiver being in charge of the connection, starts with an initial value of  65,535 octets both for new streams and the overall connection, only data frames managed by flow control, cannot be disabled, and implementations can choose any algorithm for flow control. [ 7 ] . 

A client can assign priority for a new stream via the HEADERS frame and can update it later using a PRIORITY frame. A stream is defined as “A "stream" is an independent, bidirectional sequence of frames exchanged between the client and server within an HTTP/2 connection.”- [ 7 ] . Several streams can be open concurrently and frames from multiple streams can be interleaved by the endpoints. Streams can be unilateral or shared and can be closed by either side. Each stream has an integer identifiers assigned by the initiator. Frame send order determines receive order. 

In HTTP/3, after a connection is established the settings frame is sent by each endpoint as the initial frame of their respective control stream. After this the server may begin processing request streams and sending responses.[12]. 

QUIC guarantees in-order delivery within each stream but not across streams. Data is frames onto QUIC stream frames below the framing layer and then buffered and ordered by the transport layer receiving them. Streams can be unidirectional or bidirectional initiated by either server or client. Each side initiates a control stream at the start of the session and sends its settings frame on it as the first frame [12]. The QUIC layer does the stream management of any HTTP headers and data sent via QUIC. There are other specialized stream types e.g. push streams and reserved streams.

</p></br>

  
AUTHENTICATION AND SECURITY
    <p>     
In HTTP/1.1, the message including all its headers are transferred in cleartext. This prompted the development of a security mechanism to protect the HTTP message. This began with the development of Secure-HTTP which authenticated the client and worked at the application layer[19]. S-HTTP involved encrypting the message with both the sender’s and the receiver’s keying material and adding the information about transformations into the header. Later, SHTTP gave way to SSL[20] which is a transport layer security protocol that authenticates the server. SSL preceded the current standard which is TLS. 

- Security concerns in HTTP/1.1 include[4]. 
- Attacks Based on File and Path Names
- Attacks Based on Command, Code, or Query Injection
- Disclosure of Personal Information
- Disclosure of Sensitive Information in URIs
- Disclosure of Fragment after Redirects
- Disclosure of Product Information
- Browser Fingerprinting

HTTP/2 similarly runs over TLS Security while HTTP/3 implements encryption security features at the packet level, making TLS redundant.[18].

</p></br>

  
CROSS_VERSION COMPATIBILITY
    <p>     
HTTP/1.1 is backward compatible with HTTP/0.9, 1.0. It can recognize the request line and any valid request, as well as respond appropriately with a message in the same version used by the client. It can also recognize the status line in HTTP/1.0 responses.  [14]

HTTP/2 is fully compatible with HTTP/1.1, which is important because HTTP/1.x is highly successful and is implemented in most of the existing web. HTTP/2 is designed to introduce the framing and multiplexing features while leaving the semantics and core components -- such as methods, status codes, URI’s - of the previous version intact. It does so by creating a new layer beneath HTTP in the application layer and above the transport layer for the binary framing mechanism[ 6 ] .

Similar to HTTP/2, HTTP/3 is compatible with previous versions. It operates between the frame layer and the transport layer and does not interfere with the core of HTTP. 

</p>
  
</details>

<details><summary>Closing Note</summary>
<p>
           
There are several other features not covered in this comparative review including feature extensibility details, Error Handling, Browser and Client Adoption. The three protocol versions work together, each improving upon the previous version while leaving the core functioning intact.

</p>
</details>

<details><summary>References</summary>
<p>    
  
- https://www.w3.org/Protocols/HTTP/1.0/spec.html#Purpose
  https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- https://tools.ietf.org/html/rfc7230 
- https://tools.ietf.org/html/rfc7231
- https://tools.ietf.org/html/rfc1945 
- https://hpbn.co/http2/
- https://http2.github.io/
- https://tools.ietf.org/html/draft-mbelshe-httpbis-spdy-00
- https://quicwg.org/base-drafts/draft-ietf-quic-http.html
- https://en.wikipedia.org/wiki/Text-based_protocol
- https://en.wikipedia.org/wiki/Binary_protocol 
- https://tools.ietf.org/html/draft-ietf-quic-http-16#page-4
- https://tools.ietf.org/html/draft-ietf-quic-http-16#page-10 
- https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4.4
- https://tools.ietf.org/html/draft-ietf-quic-qpack-03
- https://tools.ietf.org/html/rfc7540
- https://tools.ietf.org/html/draft-ietf-quic-transport-16 
- https://tools.ietf.org/html/draft-ietf-quic-transport-16#ref-QUIC-TLS 
- https://www.ietf.org/rfc/rfc2660.txt
- https://www.pcmag.com/encyclopedia/term/51302/shttp
  
  -- the end --
</p>
</details>
