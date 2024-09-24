# Moving CodeProject.com to a new host

The cut-down version, without the swearing, of how we moved to a new hosting centre

## Moving CodeProject.com

Every 
few years we ask ourselves "should we manage our own servers or pay someone else 
to do that for us" and every time we do the sums it's always in favour of us managing 
our own hardware. That includes licensing, bandwidth, electricity and hosting, purchasing, 
upgrading and maintaining hardware, backups, firewalls, our time, petrol, caffeine 
and those little velcro cable ties.

We spent longer than usual researching, pricing, reference checking and kicking 
the tyres of a bunch of hosting centers until we found
[one](http://www.peer1.com/) that was cost-effective, well-managed, well-thought 
out, and whose HVAC didn't include a fan lodged in the rear door to keep the servers 
cool. They even had free coffee. We were sold.

Contracts were considered, adjusted, signed and delivered and we soon had the 
keys to our new home.

## How many servers is enough?

![Don't they look comfortable](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/Old.JPG)

Our old setup (above) looks like a lot of hardware. It is, and it isn't. 
According to some comments Digg runs using 40-50 servers split between two data 
centers. The number "500" is sometimes bandied around and may have come from counting 
auxiliary servers such as backups and staging. At CodeProject.com we can run our 
entire site on a single web server and a single database server. No problem. However, 
we choose not to.

Firstly, what's "a server"? Is it a DELL 1U dual-core machine with 1GB, or is 
it a big 2-proc, 12 core with 64GB? We have both and you can guess which I love 
more. We chose to have a number (around half a dozen) super-cheap web-servers that 
enabled us to do some nice caching while also having a ton of redundancy. I can 
pull half of these servers out of the cluster to do maintenance and no one would 
notice.

We also have servers die fairly regularly. We've bought cheap, we've bought expensive, 
now we just buy for convenience and consistency. We also have a staging server, 
and a pair of general service machines that do a great deal of nothing (they are 
on the bench, so to speak), plus a backup server, a backup tape drive, paired Active 
Directory servers, DNS, mail servers and forwarders, a pair of health monitoring 
servers and of course whopping big SQL servers that sound like jet engines. A pair 
of 'em.

So what does that give us? 8 servers to actually serve the content. Another dozen 
to stand around and look useful. Kind of like a "how many servers does it take to 
screw in a light bulb" joke, only not.

## The move

![Coffee. For the love of all things holy, coffee.](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/coffeeMe.JPG)

If 
you're going to move delicate and important machinery then clearly the best thing 
to do is get up at 5AM on a Saturday morning after a big evening the night before. 
Hang on - no, that's actually the wrong way to do it but that's what we ended up 
doing, regardless. 

![Garden shears?](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/AyCarumba.JPG)

By 7AM there was light in the sky and caffeine in our veins and we started pulling 
servers and unbolting cables, sometimes the other way around, and soon we were ready 
to go. Left in the rack were a couple of brave servers who would hold the fort until 
the new location had been setup and stabilised.

Our original plan - well, one of our many original plans - was to split the servers 
into two groups: one which would go to the new centre and one that would stay. Each 
group would comprise 3 web servers and a database server, and at the point in time 
we split the groups we would turn the site on to read-only mode so that the content 
was still available, but no updates, and hence no messy re-synching would be needed.

This was a great idea right up until the point where we realised that we could 
get the whole thing done in under 4 hours and since it was possibly the last nice 
day before the winter snows set in, it would do everyone good to be not able to 
view the site and be forced to do something such as enjoy natural light. Or sleep 
in, if they had any sense. We clearly didn't.

To move the delicate and fragile servers we carefully chose Vince's truck for 
it's load bearing capacity, it's safety, it's long and illustrious pedigree in hauling 
important stuff, and because it's all we had. It also had a sliding tray cover which 
was handy since it was meant to be raining during the move and no one brought any 
tarps.

Packing

![Buckle up](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/ReadyToGo.JPG)

Arrived

![Did we forget any of them?](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/arrived.JPG)
    
Installing the old servers in the new racks was straight forward and tedious. 
Hunting down missing cables, rails, power bars, switches and that odd shaped allen 
key for getting the lids open was exciting and frustrating. Soon enough the servers 
were racked, connected and powered and we were in business.

![Locked and Loaded](https://raw.githubusercontent.com/ChrisMaunder/Moving-To-Peer1/master/docs/assets/racked.jpg)

We were alive. Total move time was 3 hours, including battling traffic.

## Now the fun begins

I cannot stand network administration. It drives me insane. The move was easy. 
The real difficulty was dealing with the Black art of Getting The Servers To Talk 
To Each Other. DNS, AD, DC, WINS - my head was hurting and the caffeine had worn 
off.

First thing we did was actually 2 weeks prior: we set the time to live (TTL) 
on our DNS to a minute, meaning we were asking DNS servers to cache our DNS entry 
for only a minute. Doing this meant when it was time to switch our address over 
to our new location it should propagate in a minute, give or take.

The Second thing we did was setup a skeleton crew of machines and equipment to 
give us something to plug into. A fancy new chasis switch, two new firewalls, and 
new metered power bars. Vince said we needed them, but I think he just wanted some 
extra blinky LED's to keep him company as he was setting up the new network. We 
scavenged out some servers putting somethings like AD and DNS in a single fault 
mode in both locations.

We flipped the switch. We held on to something firmly rooted in the ground and 
shielded our faces and bodies from the onslaught of the masses and waited.

And waited.

And kept waiting.

Hmmm.

DNS is a tricky thing if you don't update all the servers managing your DNS. 
We logged into our hosted DNS account and updated our entries there and then stood 
back, cowering under the onslaught of tens of thousands of eager members dying to 
taste the freedom of more bandwidth, more power, more ceiling space.

The little LEDs on our SQL RAID blinked a little faster, and the fans on the 
webservers swirled a little faster. We were live and the masses had found us. It 
was, in the end, quite an anti-climax because it all just sort of worked. Almost 
disappointing, really. Except that it wasn't, obviously. Disappointing, that is.

The only real problems we had came after the site was up and running. Apparently 
domain controllers don't like to be orphaned away from the Master for several weeks and then re-united. 
After some Oprah Family Reunion crossed with some Jerry Springer clash of the Twin 
Sisters moments everything was back in working order and everyone knew who they 
were supposed to be.

## Casualities

Some of our servers were close to the point where they could apply for the old 
age pension. They had had a comfortable, sometimes cushy lifestyle wrapped safely 
in a cocoon of warm, ionised air. Unfortunately not all of these servers made 
the trip through the icy Toronto morning.

Post-move was spent with Vince speaking coolly, calmly, but certainly at a 
definite volume to HP's service department. Our primary database server was 
a mess of 15k drives, 64GB's of RAM, 24 cores in 4 xeon CPU and a partridge in 
a pear tree. This was a pretty hefty purchase as we were on the bleeding edge of 
the 4 way 6 core Xeons at the time. The issues for this server seemed to slowly creep up on us after the move. 
It all started with a few random reboot's causing several brief outages. The early 
Google-style fire from the hip diagnostic pointed firmly at RAM or onboard cache 
controller.

The conversation went like this:

**Vince**: Hi, I have a G5 that's crashing. It looks like 
a DIMM is bad or the onboard cache controller is flaky, can we get it replaced?

**Support Chat**: Please update the firmware. 

After going back and fourth updating firmware then running diagnostics then 
tech support then re-installing Windows then running 
diagnostics then tech support, Vince found himself getting the first of a myriad of replacement parts two 
weeks into the situation. We were sent 2 DIMM's to replace memory on the riser cards. 
This did nothing. Actually that's not true. The machine went from crashing once a week to 
crashing and dying once a day. It was now time to go from Chat support to Phone 
support. You know the kind where you can "Reach out and Touch someone". Strangle, 
throttle, touch: it's all such a fine line.

A technician was dispatched and scheduled to show up to replace the riser cards. 
Instead a delivery guy showed up with one riser card. Nice. There are 4 riser cards, 
what one do we change out? Well let's start right to left because at this point logic 
just doesn't seem worthwhile. In all we had 4 different technicians onsite, replacing 
the Processor module / motherboard twice, RAID controllers, NIC's and a few random sticks 
of RAM.

It had now been 4 weeks of us running on our backup database 
server.

At this point Vince suggested he drive to Head Office with the server in the back 
of his truck with it mounted to a Junkyard Wars style catapult. They decided it would 
be best to send a technician out to pick it up and take it offsite for further diagnostics. 
Several days went by and, incredibly, the server was back and functional. We let 
the delicate thing chew on some SETI packets to limber up and off it went, shoe 
horned back into the datacenter tucked snugly into a new SQL Failover Cluster. 

And the actual cause of the problem? Two flaky DIMMs and a dodgy IO 
controller that wasn't able to spot the flaky DIMMs.

## Epilogue

We've been running at Peer1's new hosting centre for a few months now and 
life is, strangely, beguilingly, quiet. We've removed many of the servers we 
originally migrated and are slowly moving to a virtualised solution for our 
servers. We used to visit our old hosting centre at least every month but the 
last time I visited the servers there was snow on the ground. It's June. We had 
no power outages, no network issues, our costs have been dramatically reduced 
and not once have we had a server shutdown due to the centre overheating. That 
joke about a fan lodged in the rear door to keep the servers cool in their old 
home? That wasn't a joke.
