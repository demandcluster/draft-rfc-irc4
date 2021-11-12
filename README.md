# draft-rfc-irc4
Draft RFC for irc⁴ᴰ

Basicly rewrite rfc1459 and others and change/drop/add;

* Adds support for startrek routing language EWT₅D⁴
* New ports 6667+6697 → 3667+3697+4444
* ASCII → unicode⁴ᴰ (but that takes time so start POC with classic unicode)
* Nick lenght max 9  → 22 (bytes) (仙上主天 = 12bytes)
* Multiline(max 11+channel setting) message for unicode art
* Server channel groups with default group
* Recursive max 22 levels of diwali namespaces to join a federated channel
* etc (paged channel/users commands)

Move federation support to protocol layer see;
[one-bridge-is-not-a-matrix](https://github.com/matrix-org/matrix-appservice-irc/issues/208)


## Unicode

Everywhere, but limit channel name to ASCII charset from rfc1459.


## Channel Groups

* server named default which is default 'local'
* '/list' without group lists the server named default
* add optional group prefix to /join #channel
* group name is also limited to ASCII
* group must not contain ,.@:# (+etc)
* c++ based namespace seperator
* joining a named grouped channel needs an '@' prefix
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

For irc⁴ we limit the bi-managed server interconnect from 0 to 8 interlinks;

Example config Server4;

	matrix_board=networkA
	matrix_link_a=irc4://server2
	matrix_link_b=irc4://server7
	matrix_link_c=irc4://server8
	matrix_link_d=irc4://server9
	matrix_link_e=
	matrix_link_f=
	matrix_link_g=
	matrix_link_h=

Example config Server2 where optional federation is added.

	matrix_board=networkA
	matrix_link_a=irc4://server4
	matrix_link_b=irc4://server5
	matrix_link_c=irc4://server1
	matrix_link_d=diwali://server7.networkB
	matrix_link_e=diwali://server9.networkB
	matrix_link_f=diwali://server11.networkB
	matrix_link_g=diwali://server0.networkC
	matrix_link_h=diwali://server1.networkC


When a multiplexed irc4 link or diwali chain is created is must check its bi-sexual port schema.

So that with state-less routing the return route is created by inversing the diwali path.


### Listing Matrix Boards

A command to return the short human ASCII managed network names;

	/matrix4

### Listing Diwali Grid

A command to return the full interconnects matrix for visualizations;

	/matrix4D


### Joining Diwali Channel

In this example we have some irc networks which have multiple cross paths between them.

* networkA
* networkB
* networkC
* networkD

The user connects to networkA and joins the channel1337 but on the other side of the network;

	/diwali #channel1337
	/diwali @::#channel1337
	/diwali @local::#channel1337      // Connect to channel with a diwali path of zero length
	/diwali :0:@local::#channel1337   // Longest interconnect path within network to join
	/diwali :1:@local::#channel1337   // Travel random 1 node and join
	/diwali :9:@local::#channel1337   // Max 9
	/diwali :R:@local::#channel1337   // Path to random node in network
	
To join to a channel on an other network we use the predefined network name;
	
	/diwali :F:networkB@::#channelB   // Fastest path
	
But goal is goto slow/long, so bouncing until max depth level should be possible;

	/diwali :0:networkB:0:networkA:0:networkB:0:networkA@::#channel1337
	
Now connecting via dead-ends mirrors and diwali recursive paths can create very long routing paths;
	
	/diwali :R:networkB@:R:networkC@:R:networkD@:R:networkA:R:networkB@:R:networkC@:R:networkD@:R:networkA@local::#channel1337

And from the first server every message is routed individually so chat has constant changing 4D startrek routing rain.

NOTE: syntax is example version written without checking for the best protocol integration.


## Diwali Paths


The chat client now can display the diwali paths per user.

=== Δ∞ ==+== dīpāvalī =================+=== msg ======  
<20:01:46> |˧˥˩¹˥˩˧³˥˦˧³˨˨˧⁷˧˥˥⁰ 　　　　　　　　　　　 hello 💝 ˧˥˩⁷˩˨˧⁹ 💕  
<21:02:47> |˧˩˨¹˧˥˥²˥˩˧¹˥˦˧¹˩˨˧¹˧˩˥⁰　　　　　　　　　　　silly  
<22:03:48> |˥˩˧¹˨˨˧¹˧˩˨¹˧˨˨¹˧˥˥¹˧˩˩¹˩˥˧¹˩˨˧¹˦˦˧¹˨˨˧¹˩˩˧⁰ 　　　　　　basic  
<23:04:49> |˧˥˥¹˩˩˧¹˥˩˧²˧˩˩¹˥˩˧¹˧˥˩¹˧˩˥¹˧˥˩¹˧˩˨¹˧˦˦³˧˨˨¹˥˦˧¹˦˦˧¹˩˩˧¹˧˨˨¹˧˦˦¹˧˩˩⁰ 　♒ UTF16BE 😎  
<00:00:00> |˧˥˩⁰ 　　　　　　　　　　　　　　　OK  


## EWT₅D⁴ Encoding

Within a matrix the relative directions follow the stable axis (xyzt) of infinity and the absolute forms lines to cross reality.

Hyperqube absolute 4D top(p) and bottom(n) root axis coordinate system;

0p = ˧ ˥ ˩ → ˧˥˩  
1n = ˧ ˩ ˥ → ˧˩˥  
2p = ˧ ˥ ˦ → ˧˥˦  
3n = ˧ ˩ ˨ → ˧˩˨  
4p = ˧ ˦ ˦ → ˧˦˦  
5n = ˧ ˨ ˨ → ˧˨˨  
6p = ˧ ˥ ˥ → ˧˥˥  
7n = ˧ ˩ ˩ → ˧˩˩  

Hyperqube relative 4D left/up/forward/red=(p) and right/down/reverse/blue=(n) axis coordinate vectors;

Xp = ˥ ˩ ˧ → ˥˩˧  
Xn = ˩ ˥ ˧ → ˩˥˧  
Yp = ˥ ˦ ˧ → ˥˦˧  
Yn = ˩ ˨ ˧ → ˩˨˧  
Zp = ˦ ˦ ˧ → ˦˦˧  
Zn = ˨ ˨ ˧ → ˨˨˧  
Tp = ˥ ˥ ˧ → ˥˥˧  
Tn = ˩ ˩ ˧ → ˩˩˧  

=== Absolute 4D Coordinate System ===

　　˧˥˩ ＿＿＿＿＿＿＿＿ ˧˥˦  
　　｜＼　　　　　　　：＼  
　　｜　＼　　　　　　：　＼  
　　｜　　＼　　　　　：　　＼  
　　｜　　　˧˥˥－－－－－－－－－˧˦˦  
　　｜　　　：　　　　： 　　　 ：  
　　˧˩˥ ＿＿＿ ：＿＿＿＿˧˩˨　　　　：  
　　＼　　　： 　　　　 ＼　　　：  
　　　＼　　： 　　　　　 ＼　　：  
　　　　＼　： 　　　　　　 ＼　：  
　　　　　＼˧˩˩＿＿＿＿＿＿＿＿＿˧˨˨  

=== Relative 4D Coordinate System ====

　　　 ˩˩˧　　 ˥˦˧　　 ˦˦˧  
　　　　＼　｜　／  
　　　　　＼｜／  
　　　 ˩˥˧－－ Ｏ －－˥˩˧  
　　　　　／｜＼  
　　　　／　｜　＼  
　　　 ˨˨˧　　 ˩˨˧ 　　 ˥˥˧　  

## Credits

	    @Ω仙⁴ ꜊꜊꜊⋇꜏꜏꜏ ⁴ﷲΩ@
	      ©Δ∞ 仙上主天
	 בְּרֵאשִׁית :o: יְסוֺד :o: יִשְׂרָאֵל
