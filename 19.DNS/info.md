## Principle and Mechanism
The **Domain Name Service (DNS)** is a fundamental component of the Internet: it maps host names to IP addresses (and vice-versa), which allows the use of www.debian.org instead of 5.153.231.4

DNS records are organized in zones as each zone matches either a domain/subdomain, or an IP address range. A primary server is authoritative on the contents of a zone while secondary servers, usually hosted on separate machines provide regularly refreshed copies of the primary zone.

Each zone can contain records of various kinds (**Resource Records**).

## Resource records
*A*: IPv4 address.

*CNAME*: alias (canonical name).

*MX*: mail exchange, an email server. This information is used by other email servers to find where to send email addressed to a given address. Each MX record has a priority. The highest-priority server (with the lowest number) is tried first.

*PTR*: mapping of an IP address to a name. Such a record is stored in a “reverse DNS” zone named after the IP address range. For example, 1.168.192.in-addr.arpa is the zone containing the reverse mapping for all addresses in the 192.168.1.0/24 range.

*AAAA*: IPv6 address.

*NS*: maps a name to a name server. Each domain must have at least one NS record. These records point at a DNS server that can answer queries concerning this domain, they usually point at the primary and secondary servers for the domain. These records also allow DNS delegation; for instance, the falcot.com zone can include an NS record for  internal.falcot.com, which means that the internal.falcot.com zone is handled by another server. Of course, this server must declare an internal.falcot.com zone.
