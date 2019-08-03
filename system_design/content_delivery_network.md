# Content Delivery Network
CDNs are separate servers which host a portion of your website or content.
For example, a folder of html and javascript files for your website.
Generally the user will require these files to access your website, so this content is the first and most important thing to deliver to the user first.
Reducing the time between delivery is the reason why CDNs exist.

By hosting CDNs across many geographical locations, it allows the content of a website/service to be deliveried quickly globally.
Greatly reducing the latency in retrieving the content if your servers were only hosted in one part of the world.

## Pros
- Increases speed. Since location of the content is closer to the user, loading the website is quicker.
- Reduces load. Since other servers are giving content to the user, less processing is required on the main server.
- Increases uptime for the user. If one of the CDNs go down, another CDN can delivery that content that maybe closer.
- Better security through obscurity. Since the main server is no longer on the receiving end of the user, your data is abstracted away from potential harm.

## Pull Based CDNs
Mainly used when a user or users from a remote geographic location requests for the first time.
The closest CDN to that request will check if that content is avaliable, if its not, it will then ask the main server for that content.
Then the main server will deliver the content to the CDN, the CDN then caches that request and finally returns the content to the user.

So this is basically a process a request if asked approach.

## Push Based CDNs

## When to use CDNs?
A great scenario to use CDNs is if most of the website content is static, say images, videos, documents that do no require input from your main server.
You can think of this as having the client side of the content independent to the server side content.
CDNs could also be used to host the static files while a different architecture or set of services handles the dynamic webpages if they were requested.
