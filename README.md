# Webserv: A C++ HTTP Adventure! 🚀

Welcome to `webserv`, where we wrestle with raw TCP sockets, decode the cryptic language of HTTP, and serve web pages like it's 1999! This project is a deep dive into the fundamentals of how the web works, built from scratch in C++. No fancy libraries holding your hand here. It's just you, the C++ standard library, and the mighty `socket` API.

## So, What's a Web Server Anyway?

Ever wondered what happens when you type `google.com` into your browser and hit Enter? Magic? Elves? Close, but not quite. A web server is a program that listens for incoming network requests (from browsers, mostly) and sends back resources like HTML pages, images, or even the results of a script. It's the digital waiter of the internet, taking your order (the HTTP request) and bringing you your food (the HTTP response).

This project teaches you the nitty-gritty of the Hypertext Transfer Protocol (HTTP), the language of the web. You'll learn why we need a standardized protocol to keep the internet from descending into chaos, and how to serve data to anyone, anywhere.

## Core Components: The Fellowship of the Server

Our web server is made of several key components, each with a crucial role to play.

### The Grand Central Station: I/O Multiplexing

Imagine a restaurant with one waiter for every single customer. It would be chaos! That's what happens if you try to handle multiple clients with a simple one-thread-per-client model. It doesn't scale.

Enter **I/O Multiplexing**. We use system calls like `select()`, `poll()`, or `epoll()` to monitor multiple sockets at once. It's like having a super-powered waiter who can tell which customer is ready to order, who's ready for their check, and who's just admiring the decor, all at the same time. This allows our server to handle many clients concurrently without breaking a sweat (or spawning a million threads).

### Decoding the Matrix: Parsing HTTP Requests

An HTTP request is just a specially formatted block of text. But it's a very *important* block of text. It contains the client's deepest desires: what they want (`GET`, `POST`, `DELETE`), where to find it (the URL), and a bunch of other information in the headers.

Our `HttpRequest` parser is like a digital archaeologist, carefully excavating the request to uncover its meaning. It reads the method, the path, the HTTP version, and all the headers, and if there's a body (like from a form submission), it reads that too. If the request is malformed, we politely (or not so politely) show them the door with a `400 Bad Request` error.

### Crafting the Perfect Reply: HTTP Responses

Once we know what the client wants, it's our sacred duty to respond. The `HttpResponse` component is our master chef, cooking up the perfect response.

This involves:
1.  **Finding the right resource:** We map the request URL to a file on our server.
2.  **Setting the status code:** `200 OK` if all is well, or one of the many error codes (`404 Not Found`, `403 Forbidden`, etc.) if things go south.
3.  **Adding headers:** We add important information like `Content-Type` (so the browser knows if it's an HTML file or a cat picture) and `Content-Length`.
4.  **Sending the body:** This is the actual content of the file or the output of a CGI script.

### The Magic Portal: Common Gateway Interface (CGI)

Static files are great, but what if you want dynamic content? What if you want to run a script to, say, calculate the 10,000th digit of pi? That's where CGI comes in.

CGI is a standard way for web servers to run external scripts and send their output back to the client. Our server can handle CGI scripts (like Python or PHP). It works like this:
1.  The server receives a request for a CGI script.
2.  It sets up the environment variables the script will need (like `QUERY_STRING`).
3.  It executes the script in a separate process.
4.  It captures the script's output and sends it back to the client as the body of the HTTP response.

It's like a portal to another dimension where scripts run free!

## Configuration

The server is configured using the `config.yaml` file. This is where you can define which ports to listen on, what the root directory for your files is, default error pages, and locations that map to specific CGI scripts or have different rules.

A simple configuration might look like this:
```yaml
server:
  listen: 8080
  root: /path/to/your/www
  error_pages:
    404: /path/to/your/error_pages/404.html
  locations:
    /scripts:
      cgi_pass: /usr/bin/python
```

## Getting Started

Ready to fire up this bad boy? Here's how.

### Build

Just run `make`:
```bash
make
```
This will compile all the `.cpp` files and create the `webserv` executable.

### Run

To run the server, you need to provide the configuration file:
```bash
./webserv config.yaml
```

Now, open your favorite web browser and navigate to `http://localhost:8080` (or whatever port you specified in your config). You should see your web page!

## Conclusion

Building a web server is a journey, not a destination. It's a fantastic way to learn about networking, protocols, and the inner workings of the internet. So go ahead, tinker with the code, break things, fix them, and have fun. The web is your oyster!