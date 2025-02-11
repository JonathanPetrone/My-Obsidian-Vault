A [Content Delivery Network (CDN)](https://en.wikipedia.org/wiki/Content_delivery_network) is a (typically global) network of servers that caches and delivers content to users based on their geographic location.

When we give users a URL to an S3 object, they'll download that object from the S3 service in the region that our bucket lives in (for me, that's `us-east-2`, near Ohio in the USA).

If a user in Australia tries to download that object, they're going to have to wait for the data to travel from Ohio to Australia... and that's a long way! A CDN, like [AWS CloudFront](https://aws.amazon.com/cloudfront/), can help with that. It takes a static asset, like an image or video, and caches it on servers all over the world. When a user requests the asset, they get it from the server closest to them, which is much faster.

## Why CDN?
A CDN like CloudFront has two purposes (as far as the context of this course is concerned):

- **Speed**: Users get content from the server closest to them, which is faster than getting it from the origin server.
- **Security**: The origin server is hidden from the public internet, and only the CDN can access it. This is a security measure that can help prevent DDoS attacks and other malicious activity.

Some CDNs, like [Cloud_Flare_](https://www.cloudflare.com/), (not to be confused with Cloud_Front_) are known for their incredibly robust security features. Things like DDoS protection, Web Application Firewalls, etc.

### What Do CDNs Serve?

Images and videos are certainly common, but in reality any static asset is a good fit for a CDN. Here at Boot.dev, we use CloudFlare's CDN to serve the static assets for our frontend:

- Images
- HTML
- CSS
- JS

We deploy on their edge network, which means that our users get the initial HTML document quickly. That said, our backend server is a Go application running in a single region in the United States, so any dynamic requests to our API still have to come all the way back to the US.
