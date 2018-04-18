## Principle and Mechanism
The **Domain Name Service (DNS)** is a fundamental component of the Internet: it maps host names to IP addresses (and vice-versa), which allows the use of www.debian.org instead of 5.153.231.4

DNS records are organized in zones as each zone matches either a domain/subdomain, or an IP address range. A primary server is authoritative on the contents of a zone while secondary servers, usually hosted on separate machines provide regularly refreshed copies of the primary zone.

The DNS delegates the responsibility of assigning domain names and mapping those names to Internet resources by designating authoritative name servers for each domain. DNS consists of a tree data structure. Each node or leaf in the tree has a label and zero or more **resource records (RR)**, which hold information associated with the domain name.

The DNS hierarchy begins with the *root* domain '.' at the top and branches downward to multiple next-level domains. Domains such as **com**, **net**, **org** ... occupy the second level of the hierarchy and domains such as **example.com** occupy the third level and so on ... The top level domains are contolled by the Internet Assigned Numbers Authority(IANA) in a root zone db. http://www.iana.org/domains/root/db

Each domain name becomes registered in a central database known as the WhoIS database.

TTL is the length that a DNS record is cached on either the Resolving Server or the users own local PC.

## Domain/Zone
A domain is a collection of resource records that ends in a common name and represents an entire subtree of the DNS name space.

A zone is the partoin of a domain for which a particular nameserver is responsbile/authoritative. This may be an entire domain or just part of a domain.

## Resource records
A resource record contains a type, a TTL, a class, and data elements organized in the following format
```{r, engine='bash', count_lines}
owner-name       TTL    class     TYPE         data
www.example.com. 300    IN         A          192.168.0.2
```

**SOA**: specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.

**A**: IPv4 address.

**CNAME**: alias (canonical name).

**MX**: mail exchange, an email server. This information is used by other email servers to find where to send email addressed to a given address. Each MX record has a priority. The highest-priority server (with the lowest number) is tried first.

**PTR**: mapping of an IP address to a name. Such a record is stored in a “reverse DNS” zone named after the IP address range. For example, 1.168.192.in-addr.arpa is the zone containing the reverse mapping for all addresses in the 192.168.1.0/24 range.

**AAAA**: IPv6 address.

**NS**: maps a name to a name server. Each domain must have at least one NS record. These records point at a DNS server that can answer queries concerning this domain, they usually point at the primary and secondary servers for the domain. These records also allow DNS delegation; for instance, the falcot.com zone can include an NS record for  internal.falcot.com, which means that the internal.falcot.com zone is handled by another server. Of course, this server must declare an internal.falcot.com zone.

The **CNAME** record maps a name to another name. It should only be used when there are no other records on that name.
The **ALIAS** record maps a name to another name, but in turns it can coexist with other records on that name.

## Auhtoritative name server
An authoritative name server is a name server that only gives answers to DNS queries from data that has been configured by an original source, for example, the domain administrator or by dynamic DNS methods, in contrast to answers obtained via a query to another name server that only maintains a cache of data.

Every DNS zone must be assigned a set of authoritative name servers. 

An authoritative server indicates its status of supplying definitive answers, deemed authoritative, by setting a protocol flag, called the "Authoritative Answer" (AA) bit in its responses.[3] This flag is usually reproduced prominently in the output of DNS administration query tools, such as dig, to indicate that the responding name server is an authority for the domain name in question.[3]


## Recursive and caching name server
In theory, authoritative name servers are sufficient for the operation of the Internet. However, with only authoritative name servers operating, every DNS query must start with recursive queries at the root zone of the Domain Name System and each user system would have to implement resolver software capable of recursive operation.

To improve efficiency, reduce DNS traffic across the Internet, and increase performance in end-user applications, the Domain Name System supports DNS cache servers which store DNS query results for a period of time determined in the configuration (time-to-live) of the domain name record in question. Typically, such caching DNS servers also implement the recursive algorithm necessary to resolve a given name starting with the DNS root through to the authoritative name servers of the queried domain. With this function implemented in the name server, user applications gain efficiency in design and operation.

The combination of DNS caching and recursive functions in a name server is not mandatory; the functions can be implemented independently in servers for special purposes.



## Workflow of a DNS lookup
When a node wants to perform name resolution using a DNS server, it begins by sending queries to the servers listed in **/etc/resolv.conf** in order, until it gets a response or runs out of servers. The **host** and **dig** commands can be used for assistance.

When the query arrives at a DNS server, the server first determines whether the information being queried resides in a zone that is responsible for. If the server is an authority for the zone that the name or address being queried belongs ti, then the server responds to the client with the informatoin contained in its local zone file. This responce is referred to as an **authoritative answer**(aa). Such answers have the **aa** flag turned on in the header of the DNS response.

If the DNS server is not an authority for the record in question, but has recently got the record to answer a previous query, it may still have a copy of the record in its cache. The cache is where answers to queries are stored for a specified time, determined by a value contained in every resource record response called the **Time To Live** (TTL). If an answer exists in the server's cache, it is provided to the client. This answer will not have the aa flag set, since the server is not authoritative for the data being provided

If the DNS server is not authoritative for the name being queried and it does not possess the record in its cache, it will then attempt to retrieve the record via an iterative process known as recursion. A DNS server with an empty cache begins the recursion process by querying one of the root nameservers by IP address retrieved from its local, pre-populated root hints file. The root nameserver will then likely respond with a referral, which indicates the nameservers that are authoritative for the TLD that contains the name being queried. 
Upon receiving the referral, the DNS server will then perform another iterative query to the TLD authoritative nameserver it was referred to. Depending on whether there are further remaining delegations in the name being queried, this authoritative nameserver will either send an authoritative answer or yet another referral. This continues until an authoritative server is reached and responds with an authoritative answer. 

The final answer along with all the intermediate answers obtained prior to it, are cached by the DNS server to improve performance. If during a lookup for www.example.com the DNS server finds out that the example.com zone has authoritative nameservers, it will query those servers directly for any future queries for information in the example.com zone, rather than starting recursion again at the root nameservers. 














