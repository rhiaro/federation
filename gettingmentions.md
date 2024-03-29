# Getting mentions

This is a proposal for a protocol to find mentions (replies to, links to, interactions with) a resource.

## Background

Some people display a public list of all their received mentions (see [mentions](https://indiewebcamp.com/mentions) for what I suspect is an incomplete list). Others have private feed of mentions behind some kind of authentication (citation needed). Many people display comments/likes/etc on individual posts. Others accept/store replies, but don't display them (citation needed). This is written under the assumption we should converge on a standard way to retrieve mentions.

Use case: a client like a reader can retreive mentions for posts without relying on the posts actually displaying them in the HTML.

The site owner can delegate mentions retrieval in the same way as receiving webmentions can be delegated. On the other hand, if they are already displaying their mentions somewhere, they can just point to this.

## Protocol

### Requester discovers target endpoint

(Same as [webmention](https://indiewebcamp.com/webmention) but with `rel="mentions"`).

```http
GET /post-by-alice HTTP/1.1
Host: alice.host
```
```http
HTTP/1.1 200 OK
Link: <http://alice.host/mentions-endpoint>; rel="mentions"

<html>
<head>
...
<link href="http://alice.host/mentions-endpoint" rel="mentions" />
...
</head>
<body>
....
<a href="http://alice.host/mentions-endpoint" rel="mentions" />
...
</body>
</html>
```

The webmention endpoint is advertised in the HTTP Link header or a `<link>` or `<a>` element with `rel="mentions"`. If more than one of these is present, the HTTP Link header takes precedence, followed by the `<link>` element, and finally the `<a>` element. Clients MUST support all three options and fall back in this order.

### Requester gets mentions

```http
GET /mentions-endpoint?target=http://alice.host/post-by-alice HTTP/1.1
Host: alice.host
Accept: ...
```

The requester adds the `target` parameter to the mentions endpoint URL and fetches that.

The requester can specify their preferred Content-type with an `Accept` header. But they should probably be ready for anything. It's a wild web out there.

## Issues

## Brainstorming

* It should work with the [webmention](https://indiewebcamp.com/webmention) protocol, but does not require implementation of sending or receiving webmentions. That is, so long as you are storing mentions somewhere, it doesn't matter how they got there; they don't actually need to have been received via webmention.
* Considered reusing `rel="webmention"`, but people who delegate their webmention endpoints are forced to use the same service for getting as receiving.
* People can point back at the target page if their comments are already displayed there.