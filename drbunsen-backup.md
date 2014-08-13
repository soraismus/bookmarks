[Dr. Bunsen](/ "Dr. Bunsen.")
=============================

![image](/static/bunsen-thumb.svg)

created by [Seth Brown](http://www.drbunsen.org/about/)

-   [Archives](/archives/)
-   [Projects](https://github.com/drbunsen)
-   [About](/about/)
-   [Contact](mailto:sethbrown@drbunsen.org)
-   [Twitter](https://twitter.com/intent/user?screen_name=DrBunsen)
-   [Feed](/feed.xml "Atom feed")
-   [Search](http://nerdquery.com/?catid=19&category=18&setcss1=)

Data Backup
-----------

August 31, 2011\
 [osx](/tag/osx/) [workflow](/tag/workflow/)

After this week’s discussion on [Build and
Analyze](http://5by5.tv/buildanalyze/40) about data backup, I thought I
would take the time to explain my backup strategy. Like Dan and Marco,
my professional life revolves around data collection and computation. My
personal life is highly digitalized. Losing any of this data would be
catastrophic. I recently decided to update my backup plan to encompass
my ever increasing need for better backup functionality. With my system,
I strive for automation and simplicity to keep the backup process as
frictionless and error-proof as possible.

Before explaining my system, here are the backup software and hardware
tools I use:

-   [Daisy
    Disk](http://itunes.apple.com/us/app/daisydisk/id411643860?mt=12):
    Invaluable for forming a picture of your system and for determining
    what directories are sucking up space.\
-   [SuperDuper!](http://www.shirt-pocket.com/SuperDuper/SuperDuperDescription.html):
    The Swiss Army knife of backup—extremely useful for a variety of
    backup tasks.\
-   Time Machine: Apple's easy to use backup tool.\
-   [`rsync`](http://en.wikipedia.org/wiki/Rsync) and
    [`cron`](http://en.wikipedia.org/wiki/Cron): The special sauce for
    file syncing and automation goodness.
-   A diff tool: Handy for comparing the differences in files. I usually
    reach for vimdiff or
    [splice](https://github.com/sjl/splice.vim)—your mileage may vary.
    [Xcode's](http://developer.apple.com/xcode/) FileMerge is also a
    good choice.\
-   Cloud backup software: More on this below, but I use
    [CrashPlan](http://www.crashplan.com/). [Amazon
    S3](http://aws.amazon.com/s3/) paired with
    [Arq](http://www.haystacksoftware.com/arq/) is a great option if you
    have a small amount of data to backup or are willing to spend a bit
    more.
-   Local storage: Given the price of hard drive bays and enclosures,
    it's probably smarter to buy preconfigured hard drives bundled with
    proprietary enclosures for local backups. I've always had success
    with [LaCie Hard Drives](http://www.lacie.com/us/index.htm) for
    their reliability, aesthetics, and Mac-friendliness. However,
    [Drobo](http://drobo.com/) has always piqued my interest despite the
    obscene price.\

I see any sound backup plan minimally composed of three storage types:\

1.  Bootable backup\
2.  Local backup\
3.  Remote backup\

### My Backup System

My backup system uses a four pronged approach to prevent data loss. The
general system looks like this:

                           ___ cloud_backup___
                         /                     \
                 servers                        |
                                                |
                                                |
                old_archives                    |                  . Time Machine
                             \                  |                 .
               active_work. . networked_hd ___ Mac ___ external_hd
                             /                  .                 \ 
               music_library                    .                   photo_library
                                           bootable_hd
                                          /
                                 Mac clone

-   **Bootable Backup**\
     Having a bootable backup is probably the most superfluous piece of
    my system, but it's a wonderful luxury to have. I've lost two
    internal hard drives in the past ten years (12” G4 PowerBook circa
    2004 and 15” MacBook circa 2007 and losing my machine's hard drive
    really compromises my productivity. Replacing these drives means
    that my computer will be inoperable for days to weeks while waiting
    for repair. This can be really problematic, especially if I need
    special software or system requirements/customizations critical for
    some task.\
     A bootable backup clone ensures that when my hard drive dies, I
    will have absolutely no downtime. When my computer's internal hard
    drive goes, I can attach the bootable drive and boot a perfect clone
    of my internal HD, then work from the bootable drive as if nothing
    ever happened. What's even better is that I can actually work off
    the bootable drive attached to another computer while my machine is
    being repaired.^[1](#fn:1 "see footnote")^\
     My bootable solution utilizes the venerable SuperDuper! together
    with [LaCie’s Starck Portable External
    HD](http://www.amazon.com/gp/product/B002VDTG22/ref=as_li_ss_tl?ie=UTF8&tag=drbunsenblog-20&linkCode=as2&camp=217145&creative=399369&creativeASIN=B002VDTG22)![image](http://www.assoc-amazon.com/e/ir?t=&l=as2&o=1&a=B002VDTG22&camp=217145&creative=399369).
    The awesome thing about the Stark is that it doesn't come with the
    typical AC/DC power adaptor and requisite power and USB cords.
    Therefore, it's very portable and an ideal drive to carry while
    commuting.\
-   **External Hard Drive**\
     The least interesting piece of my setup is an external HD, which is
    split into two partitions. One partition houses my day-to-day [Time
    Machine](http://en.wikipedia.org/wiki/Time_Machine_(Mac_OS)) backups
    and the other partition houses my referenced
    [Aperture](http://www.apple.com/aperture/) photo library. I use a
    wired connection for these two applications to maximize speed
    (here’s looking at you Aperture).\
-   **Networked Hard Drive** \
     My current networked drive is a [2Tb LaCie d2 Quadra Hard
    Disk](http://www.amazon.com/gp/product/B002KPUX2S/ref=as_li_ss_tl?ie=UTF8&tag=drbunsenblog-20&linkCode=as2&camp=217145&creative=399369&creativeASIN=B002KPUX2S)![image](http://www.assoc-amazon.com/e/ir?t=&l=as2&o=1&a=B002KPUX2S&camp=217145&creative=399369).^[2](#fn:2 "see footnote")^
    This HD contains three partitions. The archive volume stores old
    archived files and directories that I don't typically need to
    access. I try to reorganize my archive volume prior to each OS
    upgrade. The second volume contains my music library, which I like
    to store on the network drive so that my music can be accessible
    throughout the house, garage, and yard. The third volume houses
    active work projects. I keep this volume in sync using a Python
    script I wrote that wraps rsync to manages server access via ssh key
    authentication. Working from this drive is useful if a server is
    down or if I want to work from home.\
-   **Cloud Backup**\
     I think everyone should probably have some form of remote backup.
    If a natural disaster such as a fire, hurricane, flood, or
    earthquake destroyed all my local data, I would still feel safe
    knowing my data was protected at an off-site data storage facility.
    I tried quite a few cloud back services and read numerous
    reviews—[here](http://shawnblanc.net/2011/06/off-site-backups/),
    [here](http://www.pcmag.com/article2/0,2817,2288745,00.asp), and
    [here](http://gigaom.com/apple/backblaze-vs-crashplan-mac-backup-smackdown-round-2/).
    Ultimately, I chose CrashPlan for their competitive pricing,
    unlimited data storage option, and the ability to backup servers
    without a graphical environment using a [headless
    client](http://support.crashplan.com/doku.php/how_to/configure_a_headless_client).
    Overall, I 'been extremely happen with their product, but I wish
    they would provide a native Mac application rather than the hideous
    java-based app currently available. I currently use CrashPlan's
    headless client to backup several servers. The setup is simple
    thanks to CrashPlan's documentation and [this
    gem](http://innerfusion.tumblr.com/post/616874889/setup-crashplan-to-manage-remote-clients-in-7-steps).
    I also backup everything illustrated above in dashed lines to
    CrashPlan (servers, old\_archives, music\_library, Mac, and
    photo\_library). The items illustrated with dots is the only data
    not sent to CrashPlan (active\_work, bootable\_hd, and Time
    Machine). Crashplan can also can send you a seeded backup of your
    data by mail should I ever lose all local data and need to get up
    and running rapidly. Given CrashPlan 's unlimited data backup model,
    it will be interesting to see if they can accommodate the growing
    size of customer backups.

* * * * *

1.  This feature is dependent on specific hardware architecture. I've
    successfully booted my MacBook Pro clone on a MacBook and another
    MacBook Pro, but not from a PowerPC G4
    PowerBook.[↩](#fnref:1 "return to article")

2.  This HD is to small for my current needs. I am eagerly awaiting an
    upgrade to a Thunderbolt-equipped HD. My understanding is that the
    Thunderbolt drives should accommodate daisy-chaining several drives
    together to my network. This should accommodate future storage
    expansion.[↩](#fnref:1 "return to article")

Powered by [Flask](http://flask.pocoo.org/). Hosted on
[S3](http://aws.amazon.com/s3/). Created by [Seth Brown](/about/).
Licensed under [CC-BY](http://creativecommons.org/licenses/by/3.0/).
