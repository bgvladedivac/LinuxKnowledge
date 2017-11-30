## Principle and Mechanism
The **Domain Name Service (DNS)** is a fundamental component of the Internet: it maps host names to IP addresses (and vice-versa), which allows the use of www.debian.org instead of 5.153.231.4

DNS records are organized in zones as each zone matches either a domain/subdomain, or an IP address range. A primary server is authoritative on the contents of a zone while secondary servers, usually hosted on separate machines provide regularly refreshed copies of the primary zone.

The DNS delegates the responsibility of assigning domain names and mapping those names to Internet resources by designating authoritative name servers for each domain. DNS consists of a tree data structure. Each node or leaf in the tree has a label and zero or more **resource records (RR)**, which hold information associated with the domain name.


## Resource records
**SOA**: specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.

**A**: IPv4 address.

**CNAME**: alias (canonical name).

**MX**: mail exchange, an email server. This information is used by other email servers to find where to send email addressed to a given address. Each MX record has a priority. The highest-priority server (with the lowest number) is tried first.

**PTR**: mapping of an IP address to a name. Such a record is stored in a “reverse DNS” zone named after the IP address range. For example, 1.168.192.in-addr.arpa is the zone containing the reverse mapping for all addresses in the 192.168.1.0/24 range.

**AAAA**: IPv6 address.

**NS**: maps a name to a name server. Each domain must have at least one NS record. These records point at a DNS server that can answer queries concerning this domain, they usually point at the primary and secondary servers for the domain. These records also allow DNS delegation; for instance, the falcot.com zone can include an NS record for  internal.falcot.com, which means that the internal.falcot.com zone is handled by another server. Of course, this server must declare an internal.falcot.com zone.

## Auhtoritative name server
An authoritative name server is a name server that only gives answers to DNS queries from data that has been configured by an original source, for example, the domain administrator or by dynamic DNS methods, in contrast to answers obtained via a query to another name server that only maintains a cache of data.

Every DNS zone must be assigned a set of authoritative name servers. 

An authoritative server indicates its status of supplying definitive answers, deemed authoritative, by setting a protocol flag, called the "Authoritative Answer" (AA) bit in its responses.[3] This flag is usually reproduced prominently in the output of DNS administration query tools, such as dig, to indicate that the responding name server is an authority for the domain name in question.[3]


## Recursive and caching name server
In theory, authoritative name servers are sufficient for the operation of the Internet. However, with only authoritative name servers operating, every DNS query must start with recursive queries at the root zone of the Domain Name System and each user system would have to implement resolver software capable of recursive operation.

To improve efficiency, reduce DNS traffic across the Internet, and increase performance in end-user applications, the Domain Name System supports DNS cache servers which store DNS query results for a period of time determined in the configuration (time-to-live) of the domain name record in question. Typically, such caching DNS servers also implement the recursive algorithm necessary to resolve a given name starting with the DNS root through to the authoritative name servers of the queried domain. With this function implemented in the name server, user applications gain efficiency in design and operation.

The combination of DNS caching and recursive functions in a name server is not mandatory; the functions can be implemented independently in servers for special purposes.











