# Day 2 How the Internet FInds a Website 

## Learning Objective 
what happens from the moment a user enters a website in the browser until the page appears on the screen.

## 1. DNS 
DNS >> Domain Name System 
COnverts the Human readable names in "Machine readable IP address"
DNS is translator

IPv4 and IPv6

Name( google) >> DNS >> IPaddress

The conversion process from Domain Name to IP address is called DNS Resolution 

**URL >> Browser Cache >> operating System Cache( WIndowS) >> Router Cache >> ISP DNS Server** >> Root DNS server(receptionist) --> .com server address >> TLS Server ( knoW Google's Authoritative server) >> Authoritative DNS Server ( holds the official DNS records for the domain) >> ISP asks Google's own DNS server and get the IP 

Response comes back 
Google DNS >> ISP DNS >> Router >> OS >> Browser

Browser → OS → Router → ISP DNS → Root → TLD → Authoritative DNS → IP Address → Website

## URL 
Uniform Resource Locator( like a complete postal Address)

URL structure
protocol://domain:port/path?query_parameters#fragment
https://www.google.com:443/search?q=chatgpt&page=1#top

QA Perspective

Testing often involves verifying:

- Different query parameters (?page=2, ?sort=price)
- URL encoding
- Invalid or missing parameters
- Correct redirects when paths change

## PORT 
A Port is a logical communication endpoint on a computer that allows different applications and services to send and receive network data.

Example
**Think of it this way:**
IP Address identifies the computer/device.
Port Number identifies the specific application or service running on that device.

**So:**
IP Address tells you "Which house?"
Port tells you "Which room inside the house?"

With Ports
Instead, connections look like this:
- 192.168.1.10:52134  → Chrome
- 192.168.1.10:52135  → Spotify
- 192.168.1.10:52136  → Zoom
- 192.168.1.10:52137  → WhatsApp

**IP + Port = Complete Address**

0–1023	>> Well-known ports	>> Standard services like HTTP, HTTPS, DNS
1024–49151	>> Registered ports	>> Applications and vendor-specific services
49152–65535	>> Dynamic/Ephemeral ports	>> Temporary ports chosen by the operating system for client connections

Port belongs to a devide and not to a browser tab 

## How This Matters in QA Testing

As a QA Engineer, you'll often encounter ports in URLs, logs, and API testing.

Examples:
http://localhost:8080
localhost = your own computer.
8080 = the application is running on port 8080.
https://api.company.com:8443
8443 = a custom HTTPS port used by that service.

When using tools like Postman or SoapUI, the port in the URL tells your request exactly which service on the server should receive it.

##Where do those CSS, JavaScript, and image files come from? Does the server send everything in one response, or does the browser make additional requests?

## Protocol
A protocol is simply a set of rules that two devices agree to follow while communicating.

HTTP - The protocol browsers use to request web pages from web servers

server and browser both speak http 

HTTPS - The browser still speaks HTTP, but before sending HTTP messages, it creates an encrypted connection using TLS (Transport Layer Security).

Every connection has:

- Source IP
- Source Port
- Destination IP
- Destination Port

These four values uniquely identify a TCP connection.

# Practical

# DevTools Practical - Observations

## Observation 1: Different websites use different HTTP versions

| Website | Protocol |
|----------|----------|
| Google | HTTP/3 (h3) |
| GitHub | HTTP/2 (h2) |
| Amazon | HTTP/2 (h2) |

**Observation:**
> Not all websites use the latest HTTP version. Google uses **HTTP/3**, while GitHub and Amazon used **HTTP/2** during this session.

---

## Observation 2: All websites responded successfully

| Website | Status Code |
|----------|-------------|
| Google | 200 OK |
| GitHub | 200 OK |
| Amazon | 200 OK *(from Service Worker)* |

**Observation:**
> All three websites returned a successful **200 OK** response, indicating that the requested page was available and loaded successfully.

---

## Observation 3: HTTP/2 and HTTP/3 use `:authority` instead of `Host`

### Examples

```text
Google  :authority: www.google.com
GitHub  :authority: github.com
Amazon  :authority: www.amazon.in
```

**Observation:**
> Instead of the traditional **Host** header used in HTTP/1.1, HTTP/2 and HTTP/3 use the **`:authority`** pseudo-header to identify the requested website.

---

## Observation 4: Amazon served the page using a Service Worker

Amazon returned:

```text
200 OK (from service worker)
```

**Observation:**
> Amazon's response was served by a **Service Worker**, showing that modern websites use browser-side caching and background scripts to improve performance and enable faster page loading.

---

## Observation 5: All websites used HTTPS

### Request URLs

```text
https://www.google.com
https://github.com
https://www.amazon.in
```

**Observation:**
> All three websites used the **HTTPS** protocol, meaning the communication between the browser and the server was encrypted using **TLS**, ensuring secure data transmission.

---

# Bonus Observations

## Different IP Addresses

**Observation:**
> Each website resolved to a different **Remote Address (IP Address)**, indicating that they are hosted on different servers or Content Delivery Networks (CDNs).

---

## Standard HTTPS Port

**Observation:**
> All websites communicated using **Port 443**, which is the standard port for secure HTTPS communication.

---

## Request Method

**Observation:**
> The browser used the **GET** request method to retrieve the main HTML document from each website.

---

## Request and Response Headers

**Observation:**
> Every request contained **Request Headers**, and every response contained **Response Headers**. These headers carry metadata such as browser information, accepted content types, caching policies, and server details.

---

## Multiple Network Requests

**Observation:**
> Loading a single webpage generated multiple network requests for resources such as HTML, CSS, JavaScript, images, fonts, and icons. This shows that a modern webpage is composed of many individual resources rather than a single HTML file.