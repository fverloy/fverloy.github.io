---
layout: post
title: Everything old is new again.
date: '2018-06-13 22:00:00'
tags:
- sunmicrosystems
- rubrik
- backup
---

My first job in IT, many moons ago, was at a company called MOTEC, it was a subcontractor of Telenet, one of the biggest ISPs in Belgium. We were responsible for building the cable based telephony systems that their customers used to make phone calls over the tv distribution network.

I got hired because I knew some things about Quadruple Amplitude Modulation (QAM) and Quadrature Phase-Shift Keying (QPSK), and a bit about Sun Solaris as well. The system we operated was developed by ECI-Telecom, a Israeli based manufacturer of telecommunications equipment, and was running mostly on a Sun Microsystems infrastructure. I spent many days, and nights (incl. New Year’s Eve 1999 in fear of the Y2K bug) in the customers Network Operations Center (NOC) looking after this system and making sure it ran smoothly.

It was there that I developed a love for Sun Microsystems (my workstation was a SUN Ultra 5) and it’s Solaris OS, things like IPS, SMF, Dtrace (which Oracle recently open-sourced under the GPL), and later ZFS (I run a FreeNAS system at home, just because), and it’s rock solid stability where a joy compared to some other OS’es at the time. Besides the technology I was also fascinated by some of the people at the company, not the least of which was it’s co-founder Andreas von Bechtolsheim (or Andy Bechtolsheim for you Americans) currently the co-founder of Arista Networks, and one of the original investors in Google. To get a sense of his many accomplishments check out the video below.

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/GjR7sRASjdo?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

He also recently presented at a Tech Field Day event (NFD16) on the future of networking, specifically the 400 Gigabit Landscape, also available on YouTube if you are inclined to watch it. One of the other co-founders of Sun Microsystems, Vinod Khosla, recently came onboard as an investor at Rubrik (where I work) through his VC firm Khosla Ventures which warmed my heart. I always felt a great respect for the company even though I later ended up competing with them in the market when I joined my first vendor company.

We all know what happened to Sun Microsystems in the end, but I’m still waiting for Jonathan Schwartz to write that Sun Microsystems book he tweeted about shortly after the acquisition by Oracle in 2010 🙂

<figure class="kg-card kg-embed-card"><blockquote class="twitter-tweet">
<p lang="en" dir="ltr">I'm thinking of writing a book about my experiences as Sun's CEO (will not be in haiku). What would you like to read about? <a href="https://twitter.com/hashtag/jonathanbook?src=hash&amp;ref_src=twsrc%5Etfw">#jonathanbook</a></p>— Jonathan Schwartz 🦉 (@OpenJonathan) <a href="https://twitter.com/OpenJonathan/status/8820705925?ref_src=twsrc%5Etfw">February 8, 2010</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</figure>

There is also an interesting article from the IEEE spectrum called [“After the Sun (Microsystems) Sets, the Real Stories Come Out”](https://spectrum.ieee.org/view-from-the-valley/tech-history/cyberspace/after-the-sun-microsystems-sets-the-real-stories-come-out) that presents some insights into what is was like at the time.

Notwithstanding the rise of Linux, there is a surprising amount of Sun Solaris (and other Unix systems, like AIX) still out there in the market running mission-critical workloads at banking, retail, telecom and other organisations worldwide. (I couldn’t find a recent market share percentage number that was freely shareable, if you do please feel free to let me know).

I would recon that most of these systems are treated as their own little (or very big) island and are handled by specialised personnel. But nevertheless there are certain aspects of IT operation that are a common requirement across all heterogeneous systems, backup and recovery being one of them. I distinctly remember the tape-driven horror show I had to deal with on a daily basis, I’ll refrain from naming the backup solution to protect the not-so innocent. To that end Rubrik recently announced support for Sun Solaris in it’s 4.2 release. (AIX was already released earlier).

**Rubrik CDM 4.2 support for Sun Solaris**

As we do for all backups we also use our SLA based approach, incremental-forever backups, archival to the Public Cloud, etc. Similar to the Linux and Windows support, we are offering a connector (RBS/Rubrik Backup Service) with low resource and operational overhead. The connector supports granular folder/file level backups via the filesets concept that integrates with SLA policies. Filesets are a logical construct based on Includes and Excludes. These are often around a direct path to a file or folder but can use wildcards for greater flexibility around what is included or excluded.

<img src="/assets/img/filesets.png">

Filesets can be customized to meet specific needs including the ability to call Pre/ Post Scripts to a.o. provide application-consistent backups. Welcome to the future! 😉

