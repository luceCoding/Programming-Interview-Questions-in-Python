### 80/20 Rule
Almost every service you are providing will have this 80/20 rule applied to it. The 80/20 rule means, 80% of the most used features will make up 20% of your possible services.
For example, Chrome browser, many people use it for the browsing, gmail, youtube, bookmarks, refresh, print feature, back button, etc... But the average user won't care much about the options, extensions, or the fact that you can open 10 tabs with one button etc...
If you think of the problem this way, you can then focus a lot of your attention on the 20% of your services. How you can setup your system design around this 20%?

If most of your traffic is concentrated in one area of your services, then you should essentially separate these into two different architectures.
For example, if an average user posts something on their feed, it may or may not be that important that your friends may get that new message 1 min from now or 5 mins from now. But if a famous celebrity were to post something, a lot more people will notice that feeds would come at different times if your design was setup this way. So this would become a real issue and one way to solve this is to dedicate a network of databases and applications for just the very famous or ones will over X million followers.
