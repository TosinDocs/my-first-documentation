---
title: What Actually Happens When You Type a URL Into Your Browser?
sidebar_label: What Happens When You Type a URL?
description: A technical walkthrough of what happens between entering a URL and seeing a webpage in your browser.
---

# What Actually Happens When You Type a URL Into Your Browser?

If you’re like me and you always have questions about everything, then you’ve probably wondered, more than once, how a web page actually works and how a simple URL becomes a whole webpage, almost instantly.

It happens really quickly, I know, depending on your internet speed ofcourse, but between the moment you push `Enter`  on your keyboard and the moment you see the page, your browser has done a lot of work. 

It has to find the right server, establish a connection, request the resource, interpret the files it gets back, and eventually turn all of that information into the pixels you see on your screen.

Let me explain from the beginning.


---

## 1. The Browser Reads the URL

Imagine you type this into your browser:

```text
https://example.com/products
```
Before the browser can show you anything, it needs to understand what you've entered.

A URL, or **Uniform Resource Locator**, tells the browser where a resource is located and how to access it.

In this example:
```text
https://example.com/products
```
There are three important parts:

- `https`: the scheme, which tells the browser which protocol to use.

- `example.com`: the domain name, which identifies the website.

- `/products`: the path, which identifies the particular resource being requested.

The domain name is convenient for us humans, but computers ultimately need an IP address to communicate with the destination server.

So before the browser can send the request, it needs to find out which IP address belongs to `example.com`.

That's where DNS comes in.

## 2. DNS Finds the Server's IP Address

The browser uses **DNS (Domain Name System)** to translate the domain name into an IP address.

You can think of DNS as the system that answers a very specific question:

>"Which IP address should I send this request to for `example.com?`"

For example, the lookup might eventually return an address such as:

```text
example.com → 93.184.216.34
```

Let's assume `93.184.216.34` is the IP address of `example.com`

Now the browser knows where to send the request. You and I see `example.com` while the browser sees `93.184.216.34`.

Don't pay attention to the exact address. It isn't important here. What matters is that the browser now knows where to send the request.

Not that DNS does not retrieve the webpage. It only helps the browser discover the network address it needs.

Once the browser has an IP address, it can start establishing a connection with the server.

## 3. The Browser Establishes a TCP Connection

For traditional HTTP connections such as `HTTP/1.1` and `HTTP/2`, the browser uses **TCP (Transmission Control Protocol)** to establish a reliable connection with the server.

TCP makes sure data can be transmitted reliably between the browser and the server.

Before the browser can send application data, TCP performs a **three-way handshake**:

```img
Browser                    Server
   |                         |
   | ------ SYN -----------> |
   |                         |
   | <----- SYN-ACK -------- |
   |                         |
   | ------ ACK -----------> |
   |                         |
```

The browser sends a `SYN` packet to start the connection.

The server responds with `SYN-ACK`, acknowledging the request and indicating that it is ready to communicate.

The browser then sends an `ACK` to confirm the connection.

Now the TCP connection is established.

But the browser still hasn't sent the actual HTTP request.

Because the URL uses `https`, there is another important step first; Security.

## 4. TLS Secures the Connection

`https` means the browser needs to communicate with the server using **TLS (Transport Layer Security)**.

TLS protects data while it travels between the browser and the server.

During the TLS handshake, the browser and server negotiate cryptographic parameters and establish the information needed to encrypt their communication.

The server also provides a digital certificate that allows the browser to verify the server's identity.

Once the TLS handshake succeeds, the browser and server have an encrypted connection.

Now the browser can safely send the HTTP request.

## 5. The Browser Sends an HTTP Request

With the connection established and secured, the browser can finally ask the server for the resource.

For our example, the request might look something like this:

```http
GET /products HTTP/1.1
Host: example.com
```

The first line tells the server several things:

```http
GET /products HTTP/1.1
```

- `GET` is the HTTP Method.

- `/products` is the requested path.

- `HTTP/1.1` specifies the HTTP version being used.

The next line identiies the host:

```http
Host: example.com
```

HTTP requests can also contain many other headers that provide additional information about the request, such as the accepted content types, cookies, and information about the browser.

At this point, the request has left the browser.

Now the server has to figure out what to do with it.

## 6. The Server Processes the Request

he server receives the HTTP request and processes it.

What happens next depends on how the website is built.

For a simple static website, the server might locate an existing HTML file and return it.

For a dynamic application, the server may need to run application code, retrieve information from a database, or perform other operations before it can generate a response.

A simplified version of that process might look like this:

```img
Browser
   |
   | HTTP request
   v
Web server
   |
   v
Application
   |
   v
Database
   |
   v
Application
   |
   v
Web server
```

For example, when you request:

```text
/products
```

the application might need to retrieve the available products from a database before generating the HTML that the browser should receive.

The important point is that the browser doesn't necessarily communicate directly with the database.

The server-side application handles that work and eventually produces a response for the browser.

Once the server has finished processing the request, it sends an HTTP response back.

## 7. The Server Sends an HTTP Response

The server's response contains information that tells the browser what happened and, usually, the requested content.

A simplified response might look like this:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>Products</title>
  </head>
  <body>
    <h1>Our Products</h1>
  </body>
</html>
```

The first line contains the HTTP status code:

```http
HTTP/1.1 200 OK
```

`200 OK` tells the browser that the request was successful.

The response also contains headers.

For example:

```http
Content-Type: text/html
```

This tells the browser that the response body contains HTML.

The body contains the actual content returned by the server.

In this case, that's an HTML document.

The browser now has something to work with.

## 8. The Browser Parses the HTML and Builds the DOM

The browser receives the HTML and begins parsing it.

For example, it might receive:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Products</title>
  </head>
  <body>
    <h1>Our Products</h1>
    <p>Explore our latest products.</p>
  </body>
</html>
```
The browser doesn't simply display this text directly on the screen.

Instead, it parses the HTML and builds a structured representation called the **DOM (Document Object Model)**.

A simplified version of the resulting structure might look like this:

```img
Document
└── html
    ├── head
    │   └── title
    │       └── "Products"
    │
    └── body
        ├── h1
        │   └── "Our Products"
        │
        └── p
            └── "Explore our latest products."
```

The DOM represents the structure and content of the document as objects that the browser and JavaScript can work with.

This distinction matters because the HTML source and the DOM are not exactly the same thing.

The HTML is the document the browser received.

The DOM is the browser's parsed representation of that document.

JavaScript can then use the DOM to read or modify the page.

But the browser still needs to figure out how those elements should look.

That's where CSS comes in.

## 9. The Browser Parses CSS and Builds the CSSOM

Webpages usually contain CSS that controls how their elements should look.

The HTML might include a stylesheet like this:

```html
<link rel="stylesheet" href="/styles.css">
```

The browser sees the stylesheet reference and requests the CSS file.

For example:
```css
h1 {
  font-size: 32px;
}

p {
  color: gray;
}
```

The browser parses the CSS and builds another internal representation called the **CSSOM (CSS Object Model)**.

The DOM describes the document's structure.

The CSSOM describes the styles that apply to that structure.

The browser needs both pieces of information to determine how the page should be displayed.

And CSS isn't necessarily the only additional resource the browser discovers while processing the page.

The HTML can reference images, JavaScript files, fonts, and other resources.

For example:

```html
<img src="/images/product.jpg">

<script src="/app.js"></script>
```

Each of these resources can trigger additional network requests.

So the original URL doesn't necessarily result in just one request.

As the browser processes the returned document, it can discover more resources that it needs to retrieve.

## 10. JavaScript Can Change the Page

If the page includes JavaScript, the browser also parses and executes that code.

For example:

```html
<script src="/app.js"></script>
```

The JavaScript can interact with the DOM.

For example:

```JavaScript
document.querySelector("h1").textContent = "Hello, Tosin!";
```

This code finds the `<h1>` element and changes its text.

JavaScript can also respond to user interactions, change styles, create or remove elements, and make additional network requests.

For example, a webpage might use JavaScript to request more products after you click a button instead of loading everything in the original HTML response.

This means the page isn't necessarily finished changing after the browser receives the initial HTML.

The browser may continue processing JavaScript, responding to interactions, and making additional requests.

Eventually, however, it needs to turn all of this information into something you can actually see.

That's the rendering process.

## 11. The Browser Renders the Page

At this point, the browser has information about:

- the document structure from the DOM

- the styles from the CSSOM

- changes made by JavaScript

- the resources required by the page

It now needs to determine what should appear on the screen.

This involves several stages;

#### Layout

First, the browser calculates the size and position of elements on the page.

For example, it needs to determine:

how wide an element should be
how tall it should be
where it should appear
how elements are positioned relative to one another

This stage is commonly referred to as **layout**.

#### Paint

Once the browser knows where everything belongs, it determines what needs to be drawn.

This includes things such as:

text
colors
backgrounds
borders
images

This stage is called **paint**.

#### Compositing

The browser can then combine different painted layers into the final image that should appear on the screen.

This stage is called **compositing**.

The result is the page you see.

## The Whole Journey

So, what happened between typing:

```text
https://example.com/products
```

and seeing the webpage?

You entered one URL.

The browser resolved where to connect, established communication with the server, requested a resource, received a response, processed the returned data, and turned that information into the pixels you see on your screen.

All from one little URL.