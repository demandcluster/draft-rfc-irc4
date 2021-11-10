# draft-rfc-irc4
Draft RFC for irc⁴

Basicly rewrite rfc1459 and others and change/add;

* Adds support for startrek routing language EWT₅D⁴
* New ports 6667+6697 → 3667+3697
* ASCII → unicode
* Nick lenght max 8  → 32 (bytes) (仙上主天 = 12bytes)
* Multiline message for unicode art
* Server channel groups with default group
* Recursive max 22 levels of diwali namespaces to join a federated channel
* etc (paged channel/users commands like /listp <offset> <limit>)


## Unicode

Everywhere, but limit channel name to ASCII charset from rfc1459.


## Channel Groups

* server named default which is default 'local'.
* '/list' without group lists the server named default.
* add optional group prefix to /join #channel
* group name is also limited to ASCII
* group must not contain ,.@:# (+etc)
* c++ based namespace seperator
* joining a named grouped channel needs an '@' prefix.
* Empty group name defaults to server named default

Example

	/join #channelA
	/join @::#channelA
	/join @local::#channelA
	/grouplist
	/list @hackers
	/join @hackers::##tty


## Federated Diwali Paths

A single IRC service is build using a spanning tree network;

	                           [ Server 15 ]  [ Server 13 ] [ Server 14]
	                                 /                \         /
	                                /                  \       /
	        [ Server 11 ] ------ [ Server 1 ]       [ Server 12]
	                              /        \          /
	                             /          \        /
	                  [ Server 2 ]          [ Server 3 ]
	                    /       \                      \
	                   /         \                      \
	           [ Server 4 ]    [ Server 5 ]         [ Server 6 ]
	            /    |    \                           /
	           /     |     \                         /
	          /      |      \____                   /
	         /       |           \                 /
	 [ Server 7 ] [ Server 8 ] [ Server 9 ]   [ Server 10 ]
	
	                                  :
	                               [ etc. ]
	                                  :
	
	                 [ Fig. 1. Format of IRC server network ]

For irc⁴ we add bi-managed interconnect limits per server to 4;

Example config Server4;

	inter_connect0=server2
	inter_connect1=server7
	inter_connect2=server8
	inter_connect3=server9

Example config Server2;

	inter_connect0=server4
	inter_connect1=server5
	inter_connect2=server1
	inter_connect3=

The federation is optional and is per bi-server management per node and limited to 4 static links.

	diwali_network=networkA
	diwali_connect0=server1.networkB
	diwali_connect1=server3.networkB
	diwali_connect2=server3.networkC
	diwali_connect3=server7.networkD

When a multiplexed server-to-server inter_connect or diwali_connect is created is must check its bi-sexual port schema.


### Listing all Diwali networks

A command to return the short human ASCII managed network names;

	/matrix4


### Joining via channel via an Diwali route

In this example we have four irc networks which have multiple cross paths between them.

* networkA (10 servers)
* networkB (40 servers)
* networkC (5 servers)
* networkD (7 servers)

The user connects to networkA and joins the channel1337 but on the other side of the network;

	/diwali #channel1337
	/diwali @::#channel1337
	/diwali @local::#channel1337      // Connect to channel with a diwali path of zero length
	/diwali :0:@local::#channel1337   // Longest interconnect path within network to join
	/diwali :1:@local::#channel1337   // Travel random 1 node and join
	/diwali :9:@local::#channel1337   // Max 9
	/diwali :R:@local::#channel1337   // Connect to random node and join
	
To join to a channel on an other network we use the predefined network name;
	
	/diwali :3:networkB@::#channelB
	
Bouncing until max depth level should be possible;

	/diwali :0:networkB:0:networkA:0:networkB:0:networkA@::#channel1337
	
Now connecting via dead-ends mirrors and diwali recursive loops can create very long routing paths;
	
	/diwali :R:networkB@:R:networkC@:R:networkD@:R:networkA:R:networkB@:R:networkC@:R:networkD@:R:networkA@local::#channel1337



## Diwali Paths


The chat client now can display the Diwali paths per user.

===========+======================+===========
<01:23:46> |
<01:23:47> |
<01:23:48> |
<01:23:49> |




