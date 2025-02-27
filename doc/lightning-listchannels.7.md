lightning-listchannels -- Command to query active lightning channels in the entire network
==========================================================================================

SYNOPSIS
--------

**listchannels** [*short\_channel\_id*] [*source*] [*destination*]

DESCRIPTION
-----------

The **listchannels** RPC command returns data on channels that are known
to the node. Because channels may be bidirectional, up to 2 objects will
be returned for each channel (one for each direction).

If *short\_channel\_id* is a short channel id, then only known channels with a
matching *short\_channel\_id* are returned.  Otherwise, it must be null.

If *source* is a node id, then only channels leading from that node id
are returned.

If *destination* is a node id, then only channels leading to that node id
are returned.

Only one of *short\_channel\_id*, *source* or *destination* can be supplied.
If nothing is supplied, data on all lightning channels known to this
node, are returned. These can be local channels or public channels
broadcast on the gossip network.

RETURN VALUE
------------

[comment]: # (GENERATE-FROM-SCHEMA-START)
On success, an object containing **channels** is returned.  It is an array of objects, where each object contains:

- **source** (pubkey): the source node
- **destination** (pubkey): the destination node
- **short\_channel\_id** (short\_channel\_id): short channel id of channel
- **public** (boolean): true if this is announced (otherwise it must be our channel)
- **amount\_msat** (msat): the total capacity of this channel (always a whole number of satoshis)
- **message\_flags** (u8): as defined by BOLT #7
- **channel\_flags** (u8): as defined by BOLT #7
- **active** (boolean): true unless source has disabled it, or it's a local channel and the peer is disconnected or it's still opening or closing
- **last\_update** (u32): UNIX timestamp on the last channel_update from *source*
- **base\_fee\_millisatoshi** (u32): Base fee changed by *source* to use this channel
- **fee\_per\_millionth** (u32): Proportional fee changed by *source* to use this channel, in parts-per-million
- **delay** (u32): The number of blocks delay required by *source* to use this channel
- **htlc\_minimum\_msat** (msat): The smallest payment *source* will allow via this channel
- **features** (hex): BOLT #9 features bitmap for this channel
- **htlc\_maximum\_msat** (msat, optional): The largest payment *source* will allow via this channel

[comment]: # (GENERATE-FROM-SCHEMA-END)

If one of *short\_channel\_id*, *source* or *destination* is supplied and no
matching channels are found, a "channels" object with an empty list is returned.

On error the returned object will contain `code` and `message` properties,
with `code` being one of the following:

- -32602: If the given parameters are wrong.

AUTHOR
------

Michael Hawkins <<michael.hawkins@protonmail.com>>.

SEE ALSO
--------

lightning-fundchannel(7), lightning-listnodes(7)

RESOURCES
---------

Main web site: <https://github.com/ElementsProject/lightning>

Lightning RFC site

-   BOLT \#7:
    <https://github.com/lightning/bolts/blob/master/07-routing-gossip.md>

[comment]: # ( SHA256STAMP:baf45b77bd2ba22e245e007b57d8e5f70d06cbf9cebf7ed1431da6a0cf6f367a)
