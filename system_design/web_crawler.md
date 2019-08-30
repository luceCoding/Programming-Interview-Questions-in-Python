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
Lets start here, we obviously would like to be able to scale as many crawlers as we want, but its sole responsibility is to just scrape whatever data it is meant to scrape.
That can be only videos, HTML, or images, each cluster will contain the same extractors and we will have multiple clusters for each content type.
Since we can add or remove any number of crawlers, we should place a load balancer in front of each cluster.
We don't know if a certain crawler may get many pages that require more processing time than others.

The URL extractor however, will act slightly different, it will further process the HTML scraper's result and get all the URLs on that page.
This will be sent to the URL manager for later distribution.

Each extractor can keep a queue of given URLs, but you would then need to keep in mind of fault tolerance, if the extractor goes down, we will lose whatever was in the queue.
This will be covered later.

Lastly, if given a link that leads to no where, we should ignore it.

# URL Manager
The URL Manager will be given a seed of URLs to start with.
It will keep a set table of the URLs it has visited so far in a database.
When given a URL from the URL extractor, it will check if this URL has been visited before.
If it hasn't, it will send the URL to the extractor cluster.

One important thing to note is that URLs can get very long.
We don't want to store very long URLs in our database.
Instead, we can hash the URLs, however, that is not always a guarantee due to collisions.
If we have to visit more pages than there are possible hash codes, this would be a problem.

# Content Manager
We don't want to store a bunch of pages with the same set of data.
So we need a way to filter the content that we recieve after we have scraped the data.
Similar to the URL manager, we will hash the content and store a hash set in another database to use for checking.
If the content hash isn't found, the content can then be stored into the data store, else rejected.

However, there is a penalty for hashing the entire page, it can take long.
Especially when the content of the pages are very large.
We could try to do a hash based on pieces of the content such as the title, headers, first few paragraphs and video links to figure out if this is new content.
This isn't 100% best solution due to collisions, this is a question you need to ask whether it is good enough to have more duplicate content or missing out on unique content if it happens that the sub-content we select for the hash are the same.

# Fault Tolerance
The URL Manager and the Content Manager should both have a master-slave architecture for better up-time.
However, if both go down at the same time, we can revive them by using the database.
Since the URL Manager will be getting a stream of URLs, hence, having a queue, it would be important to save this queue into the database.
Therefore, requiring the database to have a visited URL hash codes and unvisited set of URL links.

For the extractors, it will depend if the extractors will have a queue or not.
If they will hold a queue, it will be important to also have a master-slave architecture.
The difference is that we cannot have a database for each extractor, as we plan to have many of them.
If both master and slave of a pair of extractors go down, we will need to rely on the URL Manager to resend the URLs to another pair of extractors.
This would then require a response back from the extractors that the URL they were given is complete so the URL Manager can mark that as visited. This would require another table for pending URLs in addition to its visited and unvisited URLs.

The second design could have the extractors to not have a queue, and only process one URL at a time, when they are done, ask the URL manager for a new URL. 
This would still require a pending URL table either way if an extractor goes down while scraping a URL. 
So this would justify this design better as we can save resources over keeping a master-slave for each extractor.
