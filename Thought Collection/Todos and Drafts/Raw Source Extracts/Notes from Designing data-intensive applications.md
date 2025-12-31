# Part I. Foundations of Data Systems
## Ch. 1 - Reliable, Scalable, and Maintainable Applications

Many applications today are data-intensive, as opposed to compute-intensive. Raw CPU power is rarely a limiting factor for these applications—bigger problems are usually the amount of data, the complexity of data, and the speed at which it is changing.

A data-intensive application is typically built from standard building blocks that provide commonly needed functionality. For example, many applications need to:

- Store data so that they, or another application, can find it again later (databases) 
- Remember the result of an expensive operation, to speed up reads (caches) 
- Allow users to search data by keyword or filter it in various ways (search indexes)
- Send a message to another process, to be handled asynchronously (stream processing)
- Periodically crunch a large amount of accumulated data (batch processing)

The author's arguing that the traditional categories (database/queue/cache) are dissolving because modern tools are **hybrid by design**.

- **Old model:** Pick the right category for your access pattern (transactional data → database, async work → queue, fast reads → cache)
- **New reality:** Tools are multi-pattern by design, so the categories don't help you choose anymore

Author's calling them all "data systems" because what matters now is **which combination of guarantees you need** (durability + ordering + speed + consistency), not which historical category a tool came from.

*Examples:*
**Redis example:** Started as an in-memory cache, but added pub/sub messaging, streams, and persistence. It's now whatever access pattern you need.

**Apache Kafka:** Distributed log system that acts like a message queue (append-only writes, consumer subscriptions) but provides database-like guarantees (durable storage, replayability, ordering). Messages aren't deleted after consumption—they persist like database records.

### Three concerns that are important in most software systems:
- **Reliability** The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error). 
- **Scalability** As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth. 
- **Maintainability** Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively. 

##### Reliability
###### Hardware Faults
**Traditional approach:** add redundant hardware components to reduce failure rates.

**Why this changed:**
- More machines = proportionally more hardware failures
- Cloud platforms (like AWS) prioritize flexibility over single-machine reliability—VMs disappear without warning

**New approach:** Software fault-tolerance that tolerates losing entire machines, not just components.

**Bonus:** Enables rolling upgrades (patch one node at a time) instead of full-system downtime for reboots.

###### Software Errors
There is no quick solution to the problem of systematic faults in software. Lots of small things can help: 

- carefully thinking about assumptions and interactions in the system;
- thorough testing;
- process isolation;
- allowing processes to crash and restart;
- measuring, 
- monitoring,
- ...and analyzing system behavior in production.

###### Human ERrors
How do we make our systems reliable, in spite of unreliable humans? The best systems combine several approaches: 

- Design systems in a way that minimizes opportunities for error. For example, well-designed abstractions, APIs, and admin interfaces make it easy to do “the right thing” and discourage “the wrong thing.” However, if the interfaces are too restrictive people will work around them, negating their benefit, so this is a tricky balance to get right. 
- Decouple the places where people make the most mistakes from the places where they can cause failures. In particular, provide fully featured non-production sandbox environments where people can explore and experiment safely, using real data, without affecting real users.
- Test thoroughly at all levels.

##### Scalability
Even if a system is working reliably today, that doesn’t mean it will necessarily work reliably in the future. One common reason for degradation is increased load: perhaps the system has grown from 10,000 concurrent users to 100,000 concurrent users, or from 1 million to 10 million. Perhaps it is processing much larger volumes of data than it did before.

Scalability is the term we use to describe a system’s ability to cope with increased load. Note, however, that it is not a one-dimensional label that we can attach to a system: it is meaningless to say “X is scalable” or “Y doesn’t scale.” Rather, discussing scalability means considering questions like “If the system grows in a particular way, what are our options for coping with the growth?” and “How can we add computing resources to handle the additional load?”

###### Describing Load
First, we need to succinctly describe the current load on the system; only then can we discuss growth questions (what happens if our load doubles?). Load can be described with a few numbers which we call load parameters. The best choice of parameters depends on the architecture of your system: it may be requests per second to a web server, the ratio of reads to writes in a database, the number of simultaneously active users in a chat room, the hit rate on a cache, or something else. Perhaps the average case is what matters for you, or perhaps your bottleneck is dominated by a small number of extreme cases.

###### Describing Performance
Once you have described the load on your system, you can investigate what happens when the load increases. You can look at it in two ways: 
- When you increase a load parameter and keep the system resources (CPU, memory, network bandwidth, etc.) unchanged, how is the performance of your system affected? 
- When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged? Both questions require performance numbers, so let’s look briefly at describing the performance of a system.

It’s common to see the average response time of a service reported. But, usually it is better to use percentiles.


###### Approaches for Coping with Load

People often talk of a dichotomy between scaling up (vertical scaling, moving to a more powerful machine) and scaling out (horizontal scaling, distributing the load across multiple smaller machines). Distributing load across multiple machines is also known as a shared-nothing architecture. A system that can run on a single machine is often simpler, but high-end machines can become very expensive, so very intensive workloads often can’t avoid scaling out. In reality, good architectures usually involve a pragmatic mixture of approaches: for example, using several fairly powerful machines can still be simpler and cheaper than a large number of small virtual machines. 

Some systems are elastic, meaning that they can automatically add computing resources when they detect a load increase, whereas other systems are scaled manually (a human analyzes the capacity and decides to add more machines to the system). An elastic system can be useful if load is highly unpredictable, but manually scaled systems are simpler and may have fewer operational surprises (see “Rebalancing Partitions”).

###### Maintainability
Maintainability It is well known that the majority of the cost of software is not in its initial development, but in its ongoing maintenance—fixing bugs, keeping its systems operational, investigating failures, adapting it to new platforms, modifying it for new use cases, repaying technical debt, and adding new features. Yet, unfortunately, many people working on software systems dislike maintenance of so-called legacy systems—perhaps it involves fixing other people’s mistakes, or working with platforms that are now outdated, or systems that were forced to do things they were never intended for. Every legacy system is unpleasant in its own way, and so it is difficult to give general recommendations for dealing with them.

- **Operability** Make it easy for operations teams to keep the system running smoothly. 
- **Simplicity** Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system. (Note this is not the same as simplicity of the user interface.) 
- **Evolvability** Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as extensibility, modifiability, or plasticity.


