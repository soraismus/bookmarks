ICMP tunnel
===========

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

An **ICMP tunnel** ^[[1]](#cite_note-ptunnel-1)^ (also known as
*ICMPTX*) establishes a [covert](/wiki/Covert_channel "Covert channel")
connection between two remote computers (a client and proxy), using
[ICMP](/wiki/Internet_Control_Message_Protocol "Internet Control Message Protocol")
echo requests and reply packets. An example of this technique is
[tunneling](/wiki/Tunneling_protocol "Tunneling protocol") complete
[TCP](/wiki/Transmission_Control_Protocol "Transmission Control Protocol")
traffic over ping requests and replies.

Contents
--------

-   [1 Technical details](#Technical_details)
-   [2 Uses](#Uses)
-   [3 Mitigation](#Mitigation)
-   [4 See also](#See_also)
-   [5 References](#References)
-   [6 External links](#External_links)

Technical details[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=1 "Edit section: Technical details")]
-----------------------------------------------------------------------------------------------------------------

ICMP tunneling works by injecting arbitrary data into an echo packet
sent to a remote computer. The remote computer replies in the same
manner, injecting an answer into another ICMP packet and sending it
back. The client performs all communication using ICMP echo request
packets, while the proxy uses echo reply packets.

In theory, it is possible to have the proxy use echo request packets
(which makes implementation much easier), but these packets are not
necessarily forwarded to the client, as the client could be behind a
translated address
([NAT](/wiki/Network_address_translation "Network address translation")).
This bidirectional data flow can be abstracted with an ordinary serial
line.

This vulnerability exists because [RFC
792](http://www.ietf.org/rfc/rfc792.txt), which is IETF's rules
governing ICMP packets, allows for an arbitrary data length for any type
0 (echo reply) or 8 (echo message) ICMP packets.

Uses[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=2 "Edit section: Uses")]
---------------------------------------------------------------------------------------

ICMP tunneling can be used to bypass firewalls rules through
[obfuscation](/wiki/Obfuscation "Obfuscation") of the actual traffic.
Depending on the implementation of the ICMP tunneling software, this
type of connection can also be categorized as an encrypted communication
channel between two computers. Without proper [deep packet
inspection](/wiki/Deep_packet_inspection "Deep packet inspection") or
log review, network administrators will not be able to detect this type
of traffic through their network.^[[2]](#cite_note-2)^

Mitigation[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=3 "Edit section: Mitigation")]
---------------------------------------------------------------------------------------------------

Although the only way to prevent this type of tunneling is to block ICMP
traffic
altogether^[*[citation\\ needed](/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")*]^,
this is not realistic for a production or real-world environment. One
method for mitigation of this type of attack is to only allow fixed
sized ICMP packets through firewalls to virtually eliminate this type of
behavior.^[[3]](#cite_note-3)^

See also[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=4 "Edit section: See also")]
-----------------------------------------------------------------------------------------------

-   [ICMPv6](/wiki/ICMPv6 "ICMPv6")
-   [Smurf attack](/wiki/Smurf_attack "Smurf attack")

References[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=5 "Edit section: References")]
---------------------------------------------------------------------------------------------------

1.  **[\^](#cite_ref-ptunnel_1-0)** Daniel Stødle. ["Ping Tunnel: For
    those times when everything else is
    blocked."](http://www.cs.uit.no/~daniels/PingTunnel/).
2.  **[\^](#cite_ref-2)**
    [http://protocol.korea.ac.kr/publication/Covert%20Channel%20Detection%20in%20the%20ICMP%20Payload%20Using%20Support%20Vector%20Machine.pdf](http://protocol.korea.ac.kr/publication/Covert%20Channel%20Detection%20in%20the%20ICMP%20Payload%20Using%20Support%20Vector%20Machine.pdf)
3.  **[\^](#cite_ref-3)**
    [http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.61.5798&rep=rep1&type=pdf](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.61.5798&rep=rep1&type=pdf)

External links[[edit](/w/index.php?title=ICMP_tunnel&action=edit&section=6 "Edit section: External links")]
-----------------------------------------------------------------------------------------------------------

-   [PD-Proxy](http://www.pdproxy.com/) a VPN that can tunnel IP over
    ICMP (and also UDP or TCP)
-   [Droid-VPN](http://droidvpn.com/) an Android app that can use
    PD-Proxy servers
-   [Wi-Free](http://www.wi-free.com/) -a VPN that can tunnel IP over
    ICMP
-   [Tunnel Guru](http://web-tunnel.com/) VPN that can tunnel IP over
    ICMP
-   [Troid-VPN](http://troidvpn.com/) Android VPN that can tunnel IP
    over ICMP
-   [itun](http://sourceforge.net/projects/itun) Simple IP over ICMP
    tunnel
-   [Hans](http://code.gerade.org/hans/) ICMP tunnel for Linux (server
    and client) and BSD MacOSX (client only)
-   [BlueBitter](http://www.bluebitter.de) Proof of concept ICMP remote
    command and chat for Windows
-   [ICMP-Shell](http://icmpshell.sourceforge.net/) a telnet-like
    protocol using only ICMP
-   [PingTunnel](http://www.cs.uit.no/~daniels/PingTunnel/) Tunnel TCP
    over ICMP
-   ICMP Crafting
    [[1]](http://www.giac.org/paper/gsec/1354/icmp-crafting-issues/102553)
    by Stuart Thomas
-   Malacious ICMP Tunneling : Defense Against the Vulnerability
    [[2]](http://www.2factor.us/icmp.pdf) by Abhishek Singh
-   [RFC 792](//tools.ietf.org/html/rfc792), *Internet Control Message
    Protocol*
-   [Using the ICMP tunneling tool Ping
    Tunnel](http://neverfear.org/blog/view/9/Using_ICMP_tunneling_to_steal_Internet)
-   [Project Loki](http://phrack.org/issues.html?issue=49&id=6#article)"
    Article on ping tunneling in *[Phrack](/wiki/Phrack "Phrack")*
-   [http://IcmpTunnel.com](http://IcmpTunnel.com)
-   [http://blog.brunogarcia.com/2012/03/icmp-for-stealth-transport-of-data.html](http://blog.brunogarcia.com/2012/03/icmp-for-stealth-transport-of-data.html)
    ICMP tunnel with C\# source code

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=ICMP\_tunnel&oldid=575473385](http://en.wikipedia.org/w/index.php?title=ICMP_tunnel&oldid=575473385)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Tunneling
    protocols](/wiki/Category:Tunneling_protocols "Category:Tunneling protocols")
-   [Internet
    privacy](/wiki/Category:Internet_privacy "Category:Internet privacy")

Hidden categories:

-   [All articles with unsourced
    statements](/wiki/Category:All_articles_with_unsourced_statements "Category:All articles with unsourced statements")
-   [Articles with unsourced statements from March
    2013](/wiki/Category:Articles_with_unsourced_statements_from_March_2013 "Category:Articles with unsourced statements from March 2013")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=ICMP+tunnel&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=ICMP+tunnel "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/ICMP_tunnel "View the content page [c]")
-   [Talk](/wiki/Talk:ICMP_tunnel "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/ICMP_tunnel)
-   [Edit](/w/index.php?title=ICMP_tunnel&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=ICMP_tunnel&action=history "Past versions of this page [h]")

### Actions[](#)

### Search

![Search](//bits.wikimedia.org/static-1.23wmf11/skins/vector/images/search-ltr.png?303-4)

[](/wiki/Main_Page "Visit the main page")

### Navigation

-   [Main page](/wiki/Main_Page "Visit the main page [z]")
-   [Contents](/wiki/Portal:Contents "Guides to browsing Wikipedia")
-   [Featured
    content](/wiki/Portal:Featured_content "Featured content – the best of Wikipedia")
-   [Current
    events](/wiki/Portal:Current_events "Find background information on current events")
-   [Random article](/wiki/Special:Random "Load a random article [x]")
-   [Donate to
    Wikipedia](https://donate.wikimedia.org/wiki/Special:FundraiserRedirector?utm_source=donate&utm_medium=sidebar&utm_campaign=C13_en.wikipedia.org&uselang=en "Support us")
-   [Wikimedia Shop](//shop.wikimedia.org "Visit the Wikimedia Shop")

### Interaction

-   [Help](/wiki/Help:Contents "Guidance on how to use and edit Wikipedia")
-   [About Wikipedia](/wiki/Wikipedia:About "Find out about Wikipedia")
-   [Community
    portal](/wiki/Wikipedia:Community_portal "About the project, what you can do, where to find things")
-   [Recent
    changes](/wiki/Special:RecentChanges "A list of recent changes in the wiki [r]")
-   [Contact page](//en.wikipedia.org/wiki/Wikipedia:Contact_us)

### Tools

-   [What links
    here](/wiki/Special:WhatLinksHere/ICMP_tunnel "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/ICMP_tunnel "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=ICMP_tunnel&oldid=575473385 "Permanent link to this revision of the page")
-   [Page information](/w/index.php?title=ICMP_tunnel&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q3702356 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=ICMP_tunnel&id=575473385 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=ICMP+tunnel)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=ICMP+tunnel&oldid=575473385&writer=rl)
-   [Printable
    version](/w/index.php?title=ICMP_tunnel&printable=yes "Printable version of this page [p]")

### Languages

-   [Deutsch](//de.wikipedia.org/wiki/ICMP-Tunnel "ICMP-Tunnel – German")
-   [Русский](//ru.wikipedia.org/wiki/ICMP_%D1%82%D0%BE%D0%BD%D0%BD%D0%B5%D0%BB%D1%8C "ICMP тоннель – Russian")
-   [Edit
    links](//www.wikidata.org/wiki/Q3702356#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 2 October 2013 at 18:32.\
-   Text is available under the [Creative Commons Attribution-ShareAlike
    License](//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License)[](//creativecommons.org/licenses/by-sa/3.0/);
    additional terms may apply. By using this site, you agree to the
    [Terms of Use](//wikimediafoundation.org/wiki/Terms_of_Use) and
    [Privacy Policy.](//wikimediafoundation.org/wiki/Privacy_policy) \
     Wikipedia® is a registered trademark of the [Wikimedia Foundation,
    Inc.](//www.wikimediafoundation.org/), a non-profit organization.

-   [Privacy
    policy](//wikimediafoundation.org/wiki/Privacy_policy "wikimedia:Privacy policy")
-   [About Wikipedia](/wiki/Wikipedia:About "Wikipedia:About")
-   [Disclaimers](/wiki/Wikipedia:General_disclaimer "Wikipedia:General disclaimer")
-   [Contact Wikipedia](//en.wikipedia.org/wiki/Wikipedia:Contact_us)
-   [Developers](https://www.mediawiki.org/wiki/Special:MyLanguage/How_to_contribute)
-   [Mobile view](//en.m.wikipedia.org/wiki/ICMP_tunnel)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

