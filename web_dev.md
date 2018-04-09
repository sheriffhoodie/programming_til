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
PUT method is used to create or overwrite a resource at a particular url that is known by the client.
POST method is used to send user-generated data to the web server, ex. when a user comments on a forum or if they upload a profile picture. It can be used if you do not know the specific url of where your newly created resource should reside. POST sends data to a specific URI and expects the resource at that URI to handle the request. POST is used for annotation of existing resources, providing a block of data (such as the result of submitting a form) to a data-handling process, or extending a database through an append operation
POST method should be used to create a subordinate (child) of the resource identified  by the Request-URI
PUT requests can be sent multiple times without without changing the result (idempotent) while multiple POST requests will create a new subordinate each time (not idempotent)

### CDN
- Using a Content Delivery Network (CDN) optimizes the delivery of static assets on your site. This allows us to offload all requests for these static assets off of your web dynos, which in turn will free those dynos to handle more requests for dynamic content.
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

Sources:
https://devcenter.heroku.com/articles/using-amazon-cloudfront-cdn
https://www.cloudflare.com/learning/cdn/what-is-a-cdn/
https://www.crazyegg.com/blog/speed-up-your-website/
