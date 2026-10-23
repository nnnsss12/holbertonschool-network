# What Happens When You Type https://www.google.com in Your Browser and Press Enter?

As a software engineer, understanding the end-to-end journey of a web request is fundamental. One of the most classic and comprehensive interview questions—and a core concept in system design—is explaining what happens under the hood when a user types a URL into a web browser and hits Enter. 

Let's break down this complex journey step-by-step through the lens of networking, security, and infrastructure.

---

## 1. DNS Request (Domain Name System)
The browser needs to communicate with a server, but human-readable domain names like `www.google.com` mean nothing to network routers, which rely on IP addresses.
* **Cache Lookup:** The browser first checks its own cache, then the OS cache, router cache, and ISP DNS cache to see if the IP address is already known.
* **Recursive & Root Resolvers:** If not found locally, the request goes to a DNS Recursive Resolver (such as Google's `8.8.8.8`). The resolver queries Root Name Servers, TLD (Top-Level Domain) servers for `.com`, and finally Google's Authoritative Name Servers to resolve `www.google.com` into an IP address.

## 2. TCP/IP & The Three-Way Handshake
Now that the browser has the destination IP address, it establishes a reliable communication channel using the Transmission Control Protocol (TCP).
* **Three-Way Handshake:** 
  1. **SYN:** The client sends a synchronization packet to the server.
  2. **SYN-ACK:** The server acknowledges the request and sends back its own synchronization packet.
  3. **ACK:** The client acknowledges the server's response. 
Once complete, an active, ordered, and error-checked TCP connection is open between the user and Google's infrastructure.

## 3. Firewall
Before packets reach Google's core servers, they pass through network **Firewalls**. 
* Firewalls inspect incoming and outgoing traffic against strict security rules. They filter out malicious packets, block potential Distributed Denial of Service (DDoS) traffic, and ensure that only authorized ports (like port 443 for HTTPS) are accessible.

## 4. HTTPS/SSL Handshake
Because the URL begins with `https://`, security is paramount. Right after the TCP handshake, an **SSL/TLS handshake** takes place.
* The client and server exchange SSL certificates, verify cryptographic keys, and establish a cipher suite. This encrypts all subsequent data packets, ensuring that third parties cannot eavesdrop on or tamper with the transferred information.

## 5. Load-Balancer
At Google's scale, a single server cannot handle millions of simultaneous requests. Therefore, traffic hits a **Load-Balancer** first.
* The load-balancer acts as a traffic cop, distributing incoming requests across a cluster of backend servers using algorithms like Round Robin or Least Connections. This ensures high availability, fault tolerance, and optimal resource utilization.

## 6. Web Server (e.g., Nginx / Apache)
The request is routed by the load-balancer to a specific backend **Web Server**.
* The web server handles HTTP/HTTPS requests directly. It can serve static content (such as HTML, CSS, images, and JavaScript files) immediately, or pass dynamic requests down to the application server layer.

## 7. Application Server
For requests requiring business logic, data processing, or user-specific operations, the web server forwards the request to an **Application Server** (running environments like Python, Node.js, Java, etc.).
* The application server executes the core logic, processes user parameters, and determines what data needs to be fetched or updated.

## 8. Database
If the application requires persistent data (such as searching a database or retrieving user profiles), the application server queries the **Database** cluster (e.g., MySQL, Bigtable).
* The database executes the query, retrieves the requested records, and sends the data back up the chain to the application server, which formats it into an HTML/JSON response and sends it back through the web server, load-balancer, and encrypted tunnel to render the Google homepage on your screen.
