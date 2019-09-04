# 80/20 Rule
Almost every service you are providing will have this 80/20 rule applied to it. The 80/20 rule means, 80% of the most used features will make up 20% of your possible services or 20% of your users generate 80% of the traffic.

For example, Chrome browser, many people use it for the browsing, gmail, youtube, bookmarks, refresh, print feature, back button, etc... But the average user won't care much about the options, extensions, or the fact that you can open 10 tabs with one button etc...
If you think of the problem this way, you can then focus a lot of your attention on the 20% of your services. How you can setup your system design around this 20%?

Another case is that most of your traffic is concentrated in one area of your services, then you should separate these into two different architectures.
For example, if an average user posts something on their feed, it may or may not be that important that your friends may get that new message 1 min from now or 5 mins from now. But if a famous celebrity were to post something, a lot more people will notice that feeds would come at different times if your design was setup this way. So this would become a real issue and one way to solve this is to dedicate a network of databases and applications for just the very famous or ones will over X million followers.

Another case is that maybe 20% of your data is accessed by 80% of your users. You can then consider if it is possible to store that 20% into a memcache for high availability. While having a separate cache that would act more like a normal LRU cache for 80% of the data.

The 80/20 rule is a great thing to think about after you have laid out your design. This is when you want to further optimize the design.

# Race Conditions
Identify any part of your system that requires more than one component to share some piece of data.
For example, if two services need to book a room at a hotel, there can be a scenario that two users want to book the same room at the hotel. You would need a locking service to block/hold any other requests for a particular room.

If you are making a web crawler, you need to keep track of visited webpages.
Instead of allowing the web crawlers to manage the visited set, reverse the responsibilities and dedicate a component like a URL manager to distribute the URLs to the web crawlers while keeping track of the visited URLs.

# Shut everything down
During the interview, generally a good interviewer will start asking the question of "What if this dies?".
This is to move you to start thinking about single points of failures.

General responses would be using a master-slave architecture.
However, there is always the question "What if both die?"
This is generally the cases when a service that was processing a response, suddenly dies.
Therefore, losing the request that was being processed.

You would then need to start moving your design into a more persistent state, so you can revive the system from scratch using the data stored. Or have a second component handle the responses and resend that response if it timesout. 
The problem is that latency of the response may slow due to the need in writing to disk for every response.
This is generally a problem related to message queues stored in memory.

# Avoid Tightly Coupled Architecture
If you were making a financial account aggregator service like Mint.com.
You would likely need to interact with many financial APIs to collect data for various accounts.

You should dedicate a section of the design to this task.
Something like aggregator services and a message queue service with its own message DB for fault tolerance.
All aggregators interact with a proxy server.

What you should not do, is have the same set of aggregators interact with different banks and credit card accounts.
The role and responsibilities start getting tightly coupled at that point.
Just duplicate each design, but dedicate a set of aggregators and message queues to only interact with one financial institution. 
This makes it easier for deployment when the API changes and less risk of the new change in breaking each other services'.
Also, future proofs your system when a financial institution is no longer around.
