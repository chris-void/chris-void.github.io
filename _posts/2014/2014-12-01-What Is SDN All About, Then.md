### What Is SDN All About, Then?
[March 1, 2012 in Idle Musings](http://www.noxrepo.org/2012/03/sdn/)

What is Software Defined Networking about?  Like everything else; it depends on who you ask.  Some would say it just means OpenFlow.  Some would say it means centralized control.  Some would say it means an end to expensive, proprietary routers and the beginning of cheap commodity switches controlled by cheap commodity servers.  Some would say it’s a lot of hype which will surely all end in tears.

But since you’re asking us… here’s some of how we think about it and how it guides some of our work.

Networking has two major problems: 1) Figure out how to get data where it needs to go, and 2) Get the data there.  To use some hot terminology, we’d say that the former item is the fundamental problem of the control plane, and the second is the fundamental problem of the data plane.

As far as the data plane goes — getting data from point A to point B — that’s been a major success story.  Armed with the great abstraction of layers, people have built reliable delivery on top of unreliable global delivery on top of unreliable local delivery on top of physical delivery, and have been managing to get an ever-increasing amount of data of every sort through a series of tubes that stretches all the way around the world.

As far as the fundamental problem of the control plane… that’s been a success story too.  Routing protocols like BGP and OSPF do a tremendous job of figuring out where a packet needs to go in order to reach its destination.  But more and more, we want more and more from the control plane — isolation, mobility, quality of service, etc., etc.  While the networking community has been doing okay meeting these growing demands, it has been largely with a series of one ad-hoc solution after another.

Software Defined Networking is the emerging discipline of actually solving control plane problems in a disciplined way using — you guessed it — software.

For more insight into this point of view, check out Scott Shenker’s talk “The Future of Networking, and the Past of Protocols” [slides](http://opennetsummit.org/talks/shenkertue.pdf)
