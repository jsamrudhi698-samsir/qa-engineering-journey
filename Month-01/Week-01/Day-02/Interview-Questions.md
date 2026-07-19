# Day 2 – Networking Fundamentals: Interview Questions

---

# Interview-Level Concept

## Q. How can multiple websites use the same IP address and port?

### Answer

Multiple websites can share the same IP address and port because the browser includes the requested hostname in the connection.

- During the **TLS Handshake**, the browser sends the hostname using **Server Name Indication (SNI)** so the server can choose the correct SSL/TLS certificate.
- After the secure connection is established, the browser sends the HTTP request containing the **Host** header.

Example:

```http
Host: youtube.com
```

The web server reads the Host header and routes the request to the correct website, even though the IP address and destination port are shared.

---

# Level 1 – Must Answer Without Thinking

## 1. What happens when you type `https://google.com` into a browser?

### Answer

1. Browser parses the URL.
2. Identifies the protocol (HTTPS).
3. Extracts the domain (`google.com`).
4. Performs DNS lookup to obtain the IP address.
5. Connects to the server using the IP address on port **443**.
6. Performs the TLS handshake.
7. Sends the HTTP request.
8. Server processes the request.
9. Server sends the HTTP response.
10. Browser downloads CSS, JavaScript, images and renders the webpage.

---

## 2. What is a URL?

### Answer

A **URL (Uniform Resource Locator)** is the complete address of a resource on the Internet.

It specifies:

- Protocol
- Domain
- Path
- Query Parameters (Optional)
- Fragment (Optional)

Example:

```text
https://www.google.com/search?q=chatgpt
```

---

## 3. Difference between Domain Name and IP Address?

### Answer

### Domain Name

- Human-readable
- Example:

```text
google.com
```

### IP Address

- Machine-readable
- Example:

```text
142.250.xxx.xxx
```

DNS maps the domain name to the IP address.

---

## 4. Why do we need DNS?

### Answer

Computers communicate using IP addresses, while humans remember names.

DNS translates:

```text
google.com
↓

142.xxx.xxx.xxx
```

so browsers know which server to connect to.

---

## 5. What is an IP Address?

### Answer

An IP address uniquely identifies a device on a network so data can be routed to the correct destination.

---

## 6. What is a Port?

### Answer

A port identifies a specific application or service running on a device.

Examples:

| Port | Service |
|------|---------|
| 80 | HTTP |
| 443 | HTTPS |
| 22 | SSH |

---

## 7. Difference between IP Address and Port?

### Answer

**IP Address**

Identifies the device.

**Port**

Identifies the application running on that device.

Example:

```text
142.xxx.xxx.xxx : 443
```

- IP → Google Server
- 443 → HTTPS Service

---

## 8. What is a Protocol?

### Answer

A protocol is a standardized set of communication rules followed by two systems exchanging data.

---

## 9. Why do we need Protocols?

### Answer

Without protocols:

- Browsers and servers would use different message formats.
- Systems wouldn't understand each other.
- Reliable communication wouldn't be possible.

---

## 10. Difference between HTTP and HTTPS?

### Answer

### HTTP

- Plain text communication
- No encryption

### HTTPS

- HTTP over TLS
- Encrypts communication
- Authenticates the server
- Protects data integrity

---

# Level 2 – Frequently Asked

## 11. What port does HTTPS use?

### Answer

```text
443
```

---

## 12. What port does HTTP use?

### Answer

```text
80
```

---

## 13. Can two websites use the same port?

### Answer

Yes.

Example:

```text
google.com
↓

IP A :443

amazon.com
↓

IP B :443
```

Port **443** is the standard HTTPS port.

Different IP addresses prevent conflicts.

---

## 14. Can two websites share the same IP?

### Answer

Yes.

Using **Virtual Hosting**.

The browser sends the **Host** header so the server knows which website was requested.

---

## 15. What is the Host Header?

### Answer

The **Host** header identifies the requested website.

Example:

```http
Host: google.com
```

It allows multiple websites to be hosted on the same IP address.

---

## 16. What is SNI?

### Answer

**Server Name Indication (SNI)** is sent during the TLS handshake before the HTTP request.

It tells the server which hostname the client wants so the correct SSL/TLS certificate can be presented.

---

## 17. Why is SNI needed?

### Answer

One IP address may host multiple HTTPS websites.

Without SNI, the server wouldn't know which SSL certificate to present during the TLS handshake.

---

## 18. What happens if you enter only `google.com`?

### Answer

The browser usually assumes:

```text
https://google.com/
```

The `/` represents the **root path**.

---

## 19. What does `/` mean?

### Answer

The root resource (homepage) of the website.

---

## 20. What happens after DNS returns the IP?

### Answer

The browser connects to the server using the IP address and the appropriate destination port (443 for HTTPS).

---

# Level 3 – Reasoning Questions

## 21. If Google and YouTube share the same IP address, how does the server know which website to return?

### Answer

The browser sends either:

```http
Host: google.com
```

or

```http
Host: youtube.com
```

The web server routes the request based on the Host header.

---

## 22. Why don't browsers communicate using domain names directly?

### Answer

Routers route packets using IP addresses, not domain names.

The browser first resolves the domain into an IP address using DNS before communication begins.

---

## 23. What would happen if DNS didn't exist?

### Answer

Users would need to remember IP addresses instead of domain names.

Example:

Instead of

```text
google.com
```

you would have to type something like

```text
142.250.xxx.xxx
```

---

## 24. Why can't HTTP alone secure passwords?

### Answer

HTTP sends data without encryption.

Anyone intercepting the traffic on an untrusted network could read it.

HTTPS encrypts the communication.

---

## 25. Why is HTTPS slower than HTTP?

### Answer

HTTPS performs a TLS handshake before exchanging HTTP messages.

This introduces a small amount of additional work.

Modern TLS implementations minimize this overhead.

---

## 26. If five browser tabs open Google, will all use different destination ports?

### Answer

No.

The destination port remains:

```text
443
```

If multiple TCP connections are created, each one receives a different **source port**.

Modern browsers may also reuse a single connection using **HTTP/2** or **HTTP/3**.

---

## 27. What uniquely identifies a TCP connection?

### Answer

A TCP connection is uniquely identified by:

```text
Source IP
Source Port
Destination IP
Destination Port
```

---

# QA-Oriented Questions

## 28. A website works on `https://example.com` but not on `http://example.com`. Is that necessarily a bug?

### Answer

Not necessarily.

The application may intentionally:

- Redirect HTTP to HTTPS
- Reject HTTP requests

Always verify the expected behavior from the requirements.

---

## 29. Why is understanding the Network tab important for QA?

### Answer

It helps verify:

- Request URLs
- Request Headers
- Response Headers
- HTTP Status Codes
- Payloads
- Response Time
- Redirects
- Cookies
- API Failures

---

## 30. A page loads but images don't. Which parts of today's topics would you investigate first?

### Answer

I would check:

- Image URLs
- Protocol (HTTP vs HTTPS)
- DNS Resolution
- Network Requests in DevTools
- HTTP Status Codes (404, 403, etc.)
- Server Responses

---

# Final Interview Question

## Explain the complete lifecycle from typing `https://amazon.in` into the browser until the homepage is displayed.

### Expected Answer

The explanation should naturally include:

- URL Parsing
- Protocol
- Domain
- DNS Resolution
- IP Address
- Port
- TLS Handshake
- SNI
- HTTP Request
- Host Header
- Server Processing
- HTTP Response
- Browser Downloads CSS
- Browser Downloads JavaScript
- Browser Downloads Images
- Browser Rendering

If you can explain this complete flow **without memorizing** and understand **why each step exists**, you've achieved the learning objective for today's networking fundamentals.