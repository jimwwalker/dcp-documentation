### Mutation (0x57)

Tells the consumer that the message contains a key mutation.

The request:
* Must have extras
* Must have key
* May have value

Extra looks like:

     Byte/     0       |       1       |       2       |       3       |
        /              |               |               |               |
       |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
       +---------------+---------------+---------------+---------------+
      0| by_seqo                                                       |
       |                                                               |
       +---------------+---------------+---------------+---------------+
      8| rev seqno                                                     |
       |                                                               |
       +---------------+---------------+---------------+---------------+
     16| flags                                                         |
       +---------------+---------------+---------------+---------------+
     20| expiration                                                    |
       +---------------+---------------+---------------+---------------+
     24| lock_time                                                     |
       +---------------+---------------+---------------+---------------+
     28| Metadata Size                 | NRU           |
       +---------------+---------------+---------------+
       Total 30 bytes

The metadata is located after the items value. The size for the value is therefore bodylen - key length - metadata size - 31 (size of extra).

NRU is an internal field used by the server and may safely be ignored by other consumers.

The consumer should not send a reply to this command. The following example shows the breakdown of the message:

Example of DCP mutation if collections are not enabled.

      Byte/     0       |       1       |       2       |       3       |
         /              |               |               |               |
        |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
        +---------------+---------------+---------------+---------------+
       0| 0x80          | 0x57          | 0x00          | 0x05          |
        +---------------+---------------+---------------+---------------+
       4| 0x1f          | 0x00          | 0x02          | 0x10          |
        +---------------+---------------+---------------+---------------+
       8| 0x00          | 0x00          | 0x00          | 0x29          |
        +---------------+---------------+---------------+---------------+
      12| 0x00          | 0x00          | 0x12          | 0x10          |
        +---------------+---------------+---------------+---------------+
      16| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      20| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      24| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      28| 0x00          | 0x00          | 0x00          | 0x04          |
        +---------------+---------------+---------------+---------------+
      32| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      36| 0x00          | 0x00          | 0x00          | 0x01          |
        +---------------+---------------+---------------+---------------+
      40| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      44| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      48| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      52| 0x00          | 0x00          | 0x00          | 0x68 ('h')    |
        +---------------+---------------+---------------+---------------+
      56| 0x65 ('e')    | 0x6c ('l')    | 0x6c ('l')    | 0x6f ('o')    |
        +---------------+---------------+---------------+---------------+
      60| 0x77 ('w')    | 0x6f ('o')    | 0x72 ('r')    | 0x6c ('l')    |
        +---------------+---------------+---------------+---------------+
      64| 0x64 ('d')    |
        +---------------+
    DCP_MUTATION command
    Field        (offset) (value)
    Magic        (0)    : 0x80
    Opcode       (1)    : 0x57
    Key length   (2,3)  : 0x0005
    Extra length (4)    : 0x1f
    Data type    (5)    : 0x00
    Vbucket      (6,7)  : 0x0210
    Total body   (8-11) : 0x00000029
    Opaque       (12-15): 0x00001210
    CAS          (16-23): 0x0000000000000000
      by seqno   (24-31): 0x0000000000000004
      rev seqno  (32-39): 0x0000000000000001
      flags      (40-43): 0x00000000
      expiration (44-47): 0x00000000
      lock time  (48-51): 0x00000000
      nmeta      (52-53): 0x0000
      nru        (54)   : 0x00
    Key          (55-59): hello
    Value        (60-64): world

Example of DCP mutation if collections are enabled, the collection-ID of 555 is encoded as 0xab, 0x04

      Byte/     0       |       1       |       2       |       3       |
         /              |               |               |               |
        |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
        +---------------+---------------+---------------+---------------+
       0| 0x80          | 0x57          | 0x00          | 0x07          |
        +---------------+---------------+---------------+---------------+
       4| 0x1f          | 0x00          | 0x02          | 0x10          |
        +---------------+---------------+---------------+---------------+
       8| 0x00          | 0x00          | 0x00          | 0x29          |
        +---------------+---------------+---------------+---------------+
      12| 0x00          | 0x00          | 0x12          | 0x10          |
        +---------------+---------------+---------------+---------------+
      16| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      20| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      24| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      28| 0x00          | 0x00          | 0x00          | 0x04          |
        +---------------+---------------+---------------+---------------+
      32| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      36| 0x00          | 0x00          | 0x00          | 0x01          |
        +---------------+---------------+---------------+---------------+
      40| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      44| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      48| 0x00          | 0x00          | 0x00          | 0x00          |
        +---------------+---------------+---------------+---------------+
      52| 0x00          | 0x00          | 0x00          | 0xab          |
        +---------------+---------------+---------------+---------------+
      56| 0x04          | 0x68 ('h')    | 0x65 ('e')    | 0x6c ('l')    |
        +---------------+---------------+---------------+---------------+
      60| 0x6c ('l')    | 0x6f ('o')    | 0x77 ('w')    | 0x6f ('o')    |
        +---------------+---------------+---------------+---------------+
      64| 0x72 ('r')    | 0x6c ('l')    | 0x64 ('d')    |
        +---------------+---------------+---------------+
    DCP_MUTATION command
    Field               (offset) (value)
    Magic               (0)    : 0x80
    Opcode              (1)    : 0x57
    Key length          (2,3)  : 0x0007
    Extra length        (4)    : 0x1f
    Data type           (5)    : 0x00
    Vbucket             (6,7)  : 0x0210
    Total body          (8-11) : 0x00000029
    Opaque              (12-15): 0x00001210
    CAS                 (16-23): 0x0000000000000000
      by seqno          (24-31): 0x0000000000000004
      rev seqno         (32-39): 0x0000000000000001
      flags             (40-43): 0x00000000
      expiration        (44-47): 0x00000000
      lock time         (48-51): 0x00000000
      nmeta             (52-53): 0x0000
      nru               (54)   : 0x00
    Key [collection-ID] (55,56): 0xab, 0x4
    Key                 (57-62): hello
    Value               (63-66): world

### Returns

This message will not return a response unless an error occurs.

### Extended Attributes (XATTRs)

If a Document has XATTRs and `DCP_OPEN_INCLUDE_XATTRS` was set as part
of the [DCP_OPEN](open-connection.md) message, then the `DCP_MUTATION`
message will include the XATTRs as part of the Value payload.

The presence of XATTRs is indicated by setting the XATTR bit in the
datatype field:

    #define PROTOCOL_BINARY_DATATYPE_XATTR uint8_t(4)

See
[Document - Extended Attributes](https://github.com/couchbase/memcached/blob/master/docs/Document.md#xattr---extended-attributes)
for details of the encoding scheme.

### Extended Meta Data Section
The extended meta data section is used to send extra meta data for a particular mutation. This section is at the very end, after the value. Its length will be set in the nmeta field.
* [**Ext_Meta**](extended_meta/ext_meta_ver1.md)

### Collections Enabled

If the DCP channel is opened with collections enabled then all mutations sent
will encode the collection-ID of the key. The collection-ID is stored in byte 0
of the key as an unsigned_leb128 (uint32_t) value. Clients must decode the leb128
collection-ID to locate the document key.

### Errors

**PROTOCOL_BINARY_RESPONSE_KEY_ENOENT (0x01)**

If a stream does not exist for the vbucket specfied on this connection.

**PROTOCOL_BINARY_RESPONSE_EINVAL (0x04)**

If data in this packet is malformed or incomplete then this error is returned.

**PROTOCOL_BINARY_RESPONSE_ERANGE (0x22)**

If the by sequence number is lower than the current by sequence number contained on the consumer. The by sequence number should always be greater than the one the consumer currently has so if the by sequence number in the mutation message is less than or equal to the by sequence number on the consumer this error is returned.

**(Disconnect)**

If this message is sent to a connection that is not a consumer.
