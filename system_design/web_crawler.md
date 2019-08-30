# Assumptions
- Given a seed of urls, crawl them.
- Possible to crawl a billion pages+.
- We would like to store all data/content in each page.

# Edge Cases
- What happens if we visit the same page?
- What happens if we have the same content on different urls?
- Web page traps? Dynamically created web pages?
- What happens if we end up searching the entire web?
- Dead links?

# Components
- We will need multiple different extractors, one for video, one for images, one for HTML, and for the URLs, etc...
- We will need a way to keep track of the visited urls, so a url manager component with its own DB for redundancy will be needed.
- We will also need a way to keep track of similar webpages, something like a content mangager, also with its own DB.
- Lastly, we will need a data store.

# Extractor / Crawler
Let start here, we obviously would like to be able to scale as many crawlers as we want, but its sole responsibility is to just scrape whatever data it is meant to scrape.
That can be only videos, HTML, or images.
Since we can add or remove any number of crawlers, we should place a load balancer in front of this cluster.
We don't know if a certain crawler may get many pages that require more processing time than others.

The URL extractor however, will act slightly different, it will further process the HTML scraper's result and get all the URLs on that page.
This will be sent to the URL manager for later distribution.

The extractor can keep a queue of given URLs, but you would then need to keep in mind of fault tolerance, if the extractor goes down, we will lose whatever was in the queue.
This will be explained below.

Lastly, if given a link that does not lead to no where, we should ignore it.

# URL Manager
The URL Manager will be given a seed of URLs to start with.
It will keep a set table of the URLs it has visited so far in a database.
When given a URL from the URL extractor, it will check if this URL has been visited before.
If it hasn't, it will send the URL to the extractor cluster.

One important thing to note is that URLs can get very long.
We don't want to store very long URLs in our database.
Instead, we can hash the URLs, however, that is not always a guarantee due to collisions.
If we have to visit more pages than there are possible hash codes, this would be a problem.

# Fault Tolerance
