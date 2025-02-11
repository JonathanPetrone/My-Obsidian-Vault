REST is a set of guidelines for how to build APIs. It's not a standard, but it's a set of conventions that many people follow. Not all back-end APIs are RESTful, but many are. As a back-end developer, you'll need to know how to build RESTful APIs.

## Collections and Singletons

In REST, it's conventional to name all of your endpoints after the resource that they represent and for the name to be plural. That's why we use `POST /api/chirps` to create a new chirp instead of `POST /api/chirp`.

To get a collection of resources it's conventional to use a `GET` request to the plural name of the resource. So we are going to use `GET /api/chirps` to get all of the chirps.

To get a _singleton_, or a _single instance_ of a resource, it's conventional to use a `GET` request to the plural name of the resource, followed by the `ID` of the resource. So we are going to use `GET /api/chirps/94b7e44c-3604-42e3-bef7-ebfcc3efff8f` to get the chirp with ID `94b7e44c-3604-42e3-bef7-ebfcc3efff8f`.
