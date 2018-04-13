Assortment of topics specific to web development.

### Where to put <script> tags in HTML markup?
- Place library script such as the jQuery library in the head section.
- Place normal script in the head unless it becomes a performance/page load issue.
- Place script associated with includes, within and at the end of that include. One example of this is .ascx user controls in asp.net pages - place the script at the end of that markup.
- Place script that impacts the render of the page at the end of the body (before the body closure).
- Do NOT place script in the markup such as <input onclick="myfunction()"/> - better to put it in event handlers in your script body instead.
- If you cannot decide, put it in the head until you have a reason not to such as page blocking issues.
- Footnote: "When you need it and not prior"

### Difference between PUT and POST HTTP Requests
- PUT method is used to create or overwrite a resource at a particular url that is known by the client.
- POST method is used to send user-generated data to the web server, ex. when a user comments on a forum or if they upload a profile picture. It can be used if you do not know the specific url of where your newly created resource should reside. POST sends data to a specific URI and expects the resource at that URI to handle the request. POST is used for annotation of existing resources, providing a block of data (such as the result of submitting a form) to a data-handling process, or extending a database through an append operation
- POST method should be used to create a subordinate (child) of the resource identified  by the Request-URI
- PUT requests can be sent multiple times without without changing the result (idempotent) while multiple POST requests will create a new subordinate each time (not idempotent)

### What happens when you enter a URL into browser and hit 'Enter'?
- Browser looks up IP address first in the browser cache, then in the OS cache, then in the Router cache, then in the DNS cache
- then the ISP’s DNS server begins a recursive search from the root nameserver, through the .com top-level nameserver, to Facebook’s nameserver. Normally, the DNS server will have names of the .com nameservers in cache, and so a hit to the root nameserver will not be necessary.
- A DNS, or domain name system, is a server with a database of IP addresses and their associated hostnames. When a user types a URL into their browser, a DNS server is what translates that URL into the IP address that indicates its location online. A DNS lookup, then, is the process of a finding a specific DNS record. You can think of it as your computer looking up a number in a phone book.
- One worrying thing about DNS is that the entire domain like wikipedia.org or facebook.com seems to map to a single IP address. Fortunately, there are ways of mitigating the bottleneck:
  - Round-robin DNS is a solution where the DNS lookup returns multiple IP addresses, rather than just one. For example, facebook.com actually maps to four IP addresses.
  - Load-balancer is the piece of hardware that listens on a particular IP address and forwards the requests to other servers. Major sites will typically use expensive high-performance load balancers.
  - Geographic DNS improves scalability by mapping a domain name to different IP addresses, depending on the client’s geographic location. This is great for hosting static content so that different servers don’t have to update shared state.
- The browser sends an HTTP request to the website's server
- The GET request names the URL to fetch: “http://facebook.com/”. The browser identifies itself (User-Agent header), and states what types of responses it will accept (Accept and Accept-Encoding headers). The Connection header asks the server to keep the TCP connection open for further requests.
- The request also contains the cookies that the browser has for this domain. As you probably already know, cookies are key-value pairs that track the state of a web site in between different page requests. And so the cookies store the name of the logged-in user, a secret number that was assigned to the user by the server, some of user’s settings, etc. The cookies will be stored in a text file on the client, and sent to the server with every request.
- The website’s server responds with a permanent redirect to preferred URL (usually for SEO reasons)
- The browser sends another GET request to the redirected URL
- The website’s server receives the request, processes it, and sends back an HTML response
- The browser begins rendering the HTML document, even before it has received all of it
- The browser sends requests for embedded objects such as stylesheets, images, and JS files. the browser will send a GET request to retrieve each of these files.
- Static files, unlike dynamic files, can be cached. For example, Facebook uses a content delivery network (CDN) to distribute static content – images, style sheets, and JavaScript files. So, the files will be copied to many machines across the globe.
- Static content often represents the bulk of the bandwidth of a site, and can be easily replicated across a CDN. Often, sites will use a third-party CDN provider, instead of operating a CDN themselves. For example, Facebook’s static files are hosted by Akamai, the largest CDN provider.
- The browser continues to make AJAX requests in the background as needed

### CDNs
Using a Content Delivery Network (CDN) optimizes the delivery of static assets on your site. This allows us to offload all requests for these static assets off of your web dynos, which in turn will free those dynos to handle more requests for dynamic content.
- Benefits:
  - Improving website load times - By distributing content closer to website visitors by using a nearby CDN server (among other optimizations), visitors experience faster page loading times. As visitors are more inclined to click away from a slow-loading site, a CDN can reduce bounce rates and increase the amount of time that people spend on the site. In other words, a faster a website means more visitors will stay and stick around longer.
  - Reducing bandwidth costs - Bandwidth consumption costs for website hosting is a primary expense for websites. Through caching and other optimizations, CDNs are able to reduce the amount of data an origin server must provide, thus reducing hosting costs for website owners.
  - Increasing content availability and redundancy - Large amounts of traffic or hardware failures can interrupt normal website function. Thanks to their distributed nature, a CDN can handle more traffic and withstand hardware failure better than many origin servers.
  - Improving website security - A CDN may improve security by providing DDoS mitigation, improvements to security certificates, and other optimizations.
- Many developers make use of Amazon’s S3 service for serving static assets that have been uploaded previously, either manually or by some form of build process. Whilst this works, this is not recommended as S3 was designed as a file storage service and not for optimal delivery of files under load. Therefore, serving static assets from S3 is not recommended.
- Amazon’s CloudFront service is designed as a content delivery network to serve static assets such as CSS or JavaScript files from edge cache locations.
- Edge cache locations are groups of servers distributed around the world optimized for the high throughput serving of small static files. Using latency based DNS resolution, Amazon are able to route requests to particular assets to the closest edge cache location to the end user reducing latency, and improving speed.
  - CloudFront works from what is called a distribution. A distribution is the direct equivalent of an S3 bucket. Each distribution is assigned a domain name that is used to access your distributions assets.
  - A distribution draws its contents from an origin. The origin is where CloudFront will draw files from should it not have a copy within the distribution. Once a file has been requested from the origin CloudFront will cache that asset and return it directly to the end user.

### How to speed up a slow website
- reduce the number of HTTP requests being made by combining and minifying your HTML, CSS, and JS files
- try to asynchronous load the larger files, or defer JS file loading
- minimize the TTFB (time to first byte), the amount of time a browser has to wait before getting the first byte of data from the server, to under 200ms. can use caching for this
- consider switching to another DNS provider that has faster average lookup times
- consider upgrading your hosting option from shared hosting (share resources like CPU, RAM, and disk space with other sites on the same server) to VPS (dedicated portions of the resources, but still sharing) to a dedicated server
- use tools like Gzip to compress larger files for faster download times
- enable caching
- compress images using tools like Pingdom and compressor.io, crop and re-size images to be their actual display sizes, to cut down on editing time
- use a CDN for static files, but for videos, use Youtube or Vimeo to host and then embed the link, to cut back on the large video file size
- avoid using inline CSS in HTML doc and minimize number of CSS files to download
- use lazy loading to prioritize loading content that is within the user’s viewport, and loading content outside the viewport afterwards
- limit the number of redirects that occur in your site, to limit the number of requests that are made
- add database indices to optimize common queries
- monitor event listeners to weed out any zombies

### Strategies to speed up Webpack build times and minimize Webpack builds

- Use packages that allows webpack to parallelize the build, i.e. spread out the work to multiple processors. Examples include “parallel-webpack”, “happypack” and the parallel option on UglifyJS plugin.
- Also in the Uglify plugin, can speed up minification by 3-4 times by disabling the compression (whitespace removal and symbol mangling)
- Skip parsing: You can tell web pack to skip certain files or sets of files in its JS file parsing process if you’re certain those files don’t use any dependencies
- More exclusions: You can also exclude files from loaders and other plugins such as tools like transpires and minifiers that also rely on syntax trees - it is common to skip minification for non-customer facing code.
- Sharing Code: To avoid having duplicate code in different bundles, can find duplicates with web pack Bundle Analyzer and Bundle Buddy and then split them out into shared chunks with web pack’s CommonsChunkPlugin.
- Create a manifest chunk: First start with a digest, a simple map of module IDs to hashes that repack will use to resolve a filename when importing modules asynchronously. Then, you need to extract this module digest into a spearte file entirely so that the boilerplate code at the top of each bundle doesn’t have to be updated. This is where you can create a CommonsChunk plugin, which greatly reduced the frequency of rebuilds and allows you to ship only a single copy of webpack’s boilerplate code.
- Research lighter, cheaper source map options.
- Remember to clear your cache whenever package dependencies change - something you can automate with an npm post install script

### Difference between SASS, LESS, and CSS?
Syntactically Awesome Stylesheets (Sass) and Leaner CSS (LESS) are both CSS preprocessors. They are special stylesheet extensions that make designing easier and more efficient. Both Sass and LESS compile into CSS stylesheets so that browsers can read them, which is a necessary step because modern browsers cannot read .sass or .less file types. When it comes down to it, both are similar. They make writing CSS simpler, more object-oriented, and a more enjoyable experience.
Differences:
- Sass is in Ruby, Less is in JavaScript
- Less has more accurate error messages
- Sass’s extension Compass for mixins (ability to store and share CSS declarations across a website) is better
- Sass uses $ for variable assignment, Less uses @

Sources:
https://devcenter.heroku.com/articles/using-amazon-cloudfront-cdn,
https://www.cloudflare.com/learning/cdn/what-is-a-cdn/,
https://www.crazyegg.com/blog/speed-up-your-website/,
http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/comment-page-3/,
https://www.thebalance.com/sass-vs-less-2071912
