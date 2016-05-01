Linux Notes

1. Daemon: The term daemon is derived from the daemons of Greek mythology, which were supernatural beings that ranked between gods and mortals and which possessed special knowledge and power. 

The term was then used to describe background processes which worked tirelessly to perform system chores. The first computer daemon was a program that automatically made tape backups. After the term was adopted for computer use, it was rationalized as an acronym for Disk And Execution MONitor.

Daemons are usually instantiated as processes. A process is an executing (i.e., running) instance of a program. Processes are managed by the kernel (i.e., the core of the operating system), which assigns each a unique process identification number (PID).

There are three basic types of processes in Linux: interactive, batch and daemon. Interactive processes are run interactively by a user at the command line (i.e., all-text mode). Batch processes are submitted from a queue of processes and are not associated with the command line; they are well suited for performing recurring tasks when system usage is otherwise low.


2. Proxy Server: In computer networks, a proxy server is a server (a computer system or an application) that acts as an intermediary for requests from clients seeking resources from other servers. A client connects to the proxy server, requesting some service, such as a file, connection, web page, or other resource available from a different server and the proxy server evaluates the request as a way to simplify and control its complexity. Proxies were invented to add structure and encapsulation to distributed systems.[1] Today, most proxies are web proxies, facilitating access to content on the World Wide Web and providing anonymity.

A proxy server may reside on the user's local computer, or at various points between the user's computer and destination servers on the Internet.
A proxy server that passes requests and responses unmodified is usually called a gateway or sometimes a tunneling proxy.
A forward proxy is an Internet-facing proxy used to retrieve from a wide range of sources (in most cases anywhere on the Internet).
A reverse proxy (or surrogate) is usually an internal-facing proxy used as a front-end to control and protect access to a server on a private network. A reverse proxy commonly also performs tasks such as load-balancing, authentication, decryption or caching. Reverse proxies are installed in the neighborhood of one or more web servers. All traffic coming from the Internet and with a destination of one of the neighborhood's web servers goes through the proxy server. The use of "reverse" originates in its counterpart "forward proxy" since the reverse proxy sits closer to the web server and serves only a restricted set of websites. 

Encryption / SSL acceleration: when secure web sites are created, the SSL encryption is often not done by the web server itself, but by a reverse proxy that is equipped with SSL acceleration hardware. See Secure Sockets Layer. Furthermore, a host can provide a single "SSL proxy" to provide SSL encryption for an arbitrary number of hosts; removing the need for a separate SSL Server Certificate for each host, with the downside that all hosts behind the SSL proxy have to share a common DNS name or IP address for SSL connections. This problem can partly be overcome by using the SubjectAltName feature of X.509 certificates.
Load balancing: the reverse proxy can distribute the load to several web servers, each web server serving its own application area. In such a case, the reverse proxy may need to rewrite the URLs in each web page (translation from externally known URLs to the internal locations).
Serve/cache static content: A reverse proxy can offload the web servers by caching static content like pictures and other static graphical content.
Compression: the proxy server can optimize and compress the content to speed up the load time.
Spoon feeding: reduces resource usage caused by slow clients on the web servers by caching the content the web server sent and slowly "spoon feeding" it to the client. This especially benefits dynamically generated pages.
Security: the proxy server is an additional layer of defense and can protect against some OS and Web Server specific attacks. However, it does not provide any protection from attacks against the web application or service itself, which is generally considered the larger threat.
Extranet Publishing: a reverse proxy server facing the Internet can be used to communicate to a firewall server internal to an organization, providing extranet access to some functions while keeping the servers behind the firewalls. If used in this way, security measures should be considered to protect the rest of your infrastructure in case this server is compromised, as its web application is exposed to attack from the Internet.

3. Payload: Payload in computing (sometimes referred to as the actual or body data) is the cargo of a data transmission. It is the part of the transmitted data which is the fundamental purpose of the transmission. Payload does not include information sent with it such as headers or metadata, sometimes referred to as overhead data, sent solely to facilitate payload delivery.
In computer security, payload refers to the part of malware which performs a malicious action.[3] In the analysis of malicious software such as worms, viruses and Trojans, it refers to the software's harmful results. Examples of payloads include data destruction, messages with insulting text or spurious e-mail messages sent to a large number of people.
In summary, payload refers to the actual intended message in a transmission.


4. Network packet: A network packet is a formatted unit of data carried by a packet-switched network. A packet consists of control information and user data, which is also known as the payload. Control information provides data for delivering the payload, for example: source and destination network addresses, error detection codes, and sequencing information. Typically, control information is found in packet headers and trailers.

5. CIDR: classless inter-domain routing, is a method for allocating IP addresses and routing Internet Protocol packets.




