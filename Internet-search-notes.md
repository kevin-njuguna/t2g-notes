# What Happens When You Type `google.co` and Press Enter?

In the realm of software engineering, understanding the underlying mechanisms that power our daily internet interactions is crucial. When you type https://www.google.com into your browser and hit Enter, a fascinating journey begins - transcending various layers of the web stack before the desired webpage graces your screen. Let's delve into this journey, demystifying the complexities involved.

---

## 1. Browser Input Handling

- A browser is essentially an application that allows you to access the internet. Some common examples include Google Chrome, Firefox, Vivaldi, Safari, Opera, Edge, etc.

- The browser checks what you typed. Since `google.co` lacks a scheme (`http://` or `https://`), most modern browsers **assume `https://`** by default.
- So the browser rewrites the address internally to:
  ```
  https://google.co/
  ```

---

## 2. Browser Cache Check

- Before making any network requests, the browser checks:

  - **DNS cache** (The DNS cache (also known as DNS resolver cache) is a temporary DNS storage on a device (your computer, smartphone, server, etc.) that contains DNS records of already visited domain names (A records for IPv4 addresses, AAAA records for IPv6, etc.).)

  - **HTTP cache** (stored content from earlier visits).

- If the resource is fresh and valid, it could be served directly from cache. The HTTP cache stores a response associated with a request and reuses the stored response for subsequent requests.

---

## 3. DNS Resolution

If the local DNS resolver does not have the most recent copy of the DNS record, it sends a request to a root nameserver. The root nameserver replies with the address of a top-level domain (TLD) nameserver, such as .com

- The local DNS resolver sends a request to the TLD nameserver.
- The TLD nameserver responds with the address of the authoritative nameserver for the domain.
- The local DNS resolver sends a request to the authoritative nameserver.
- The authoritative nameserver responds with the IP address for the domain.
- The local DNS resolver sends the IP address back to the browser.
- The browser sends a request to the server at the IP address to retrieve the webpage.

This process may involve additional steps if the DNS record is not found at any of the nameservers or if the DNS record is set up to use a service such as DNS load balancing or content delivery networks (CDN).

The duration for which the DNS record is cached (known as the "TTL" or "Time To Live") is determined by the authoritative nameserver and can be customized by the domain owner.

---

## 4. TCP Connection (3-Way Handshake)

- Your browser initiates a **TCP connection** to the server's IP over port **443** (HTTPS).
- The 3-way handshake is a fundamental process that establishes a reliable connection between two devices over a TCP/IP network. It involves three steps: SYN (Synchronize), SYN-ACK (Synchronize-Acknowledge), and ACK (Acknowledge). During the handshake, the client and server exchange initial sequence numbers and confirm the connection establishment.

In our case:

```
Client → SYN → Server
Server → SYN-ACK → Client
Client → ACK → Server
```

---

## 5. TLS Handshake (SSL)

- Since HTTPS is used, a **TLS handshake** begins:

  - Browser verifies server's **SSL certificate**.
  - Secure keys are exchanged to establish an **encrypted channel**.
  - This ensures **data confidentiality and integrity**.

- SSL/TLS are like special codes used to lock and unlock the box. When you use HTTPS to send a message, you and the recipient agree on the code to use to lock and unlock the box. This way, only you and the recipient know the code and can read the message.

---

## 6. HTTP Request

-When you enter "google.com" into your browser, the browser is like the sender of the message. The server hosting google.com is like the recipient. The browser sends a request for the webpage using HTTPS, which is like putting the request in a locked box and sending it to the server. The server then sends the webpage back to the browser using HTTPS, which is like putting the webpage in the locked box and sending it back to the browser.

---

## 7. Load balancer

- A load balancer is a tool that distributes incoming network traffic across a group of servers or resources. Its main function is to ensure that the traffic is distributed evenly across the servers to prevent overloading any single server and to increase the overall capacity and reliability of the system.

- Google requires many servers to serve all users since it receives billions of website visitiors a day.

- When a browser tries to access google.com, the load balancer receives the incoming request from the browser and forwards it to one of the servers in the Google server network. The server chosen will depend on the type of load balancing algorithm implemented.

## 7. Server-Side Processing

A web server is a software that is in charge of managing requests for web pages from clients (such as a browser attempting to access google.com). When a client sends a request for a web page to a website server, the server handles the request and returns the appropriate response to the client.

## Where applicable, the server may redirect the user.

## 8. HTTP Redirect (if applicable)

- If redirected, the browser receives:
  ```http
  HTTP/1.1 301 Moved Permanently
  Location: https://www.google.com/
  ```
- The browser now repeats:
  - DNS resolution (if not cached),
  - TCP + TLS handshake,
  - HTTP request to `https://www.google.com/`.

---

## 9. Response Received

When a browser receives a response from a website server, it processes the HTML, CSS, and JavaScript files that are included in the response in order to display the web page. The displaying process involves interpreting the HTML and CSS code, displaying any images or other media that are included on the page, and executing any JavaScript code that is present on the page.

- The HTML, CSS, JavaScript, and other assets (images, fonts) are downloaded via additional HTTP(S) requests.

---

## 10. Browser Rendering

After recieving the response from the website server, the browser utilizes these files to display the webpage and present it to you.

This method includes the following:

- Presenting the text and pictures on the webpage in the appropriate positions
- Arranging the text and design in line with the CSS styles
- Performing any Javascript code that exists on the webpage
- After the webpage has been entirely displayed, you can now engage with it..This is by: pressing links, typing text or interactiong with other features on the webpage.
