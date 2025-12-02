
Search :

with Number of packet  --> frame.number == num

with domain >> http.host == domain name

|                              |                                                                                                                                                             |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Filter                       | Description                                                                                                                                                 |
| `ip`                         | Show all IP packets.                                                                                                                                        |
| `ip.addr == 10.10.10.111`    | Show all packets containing IP address 10.10.10.111.                                                                                                        |
| `ip.addr == 10.10.10.0/24`   | Show all packets containing IP addresses from 10.10.10.0/24 subnet.                                                                                         |
| `ip.src == 10.10.10.111`     | Show all packets originated from 10.10.10.111                                                                                                               |
| `ip.dst == 10.10.10.111`     | Show all packets sent to 10.10.10.111                                                                                                                       |
| ip.addr **vs** ip.src/ip.dst | **Note:** The ip.addr filters the traffic without considering the packet direction. The ip.src/ip.dst filters the packet depending on the packet direction. |
## TCP and UDP Filters

|                       |                                                 |                       |                                                 |
| --------------------- | ----------------------------------------------- | --------------------- | ----------------------------------------------- |
| Filter                | Description                                     | Filter                | Expression                                      |
| `tcp.port == 80`      | Show all TCP packets with port 80               | `udp.port == 53`      | Show all UDP packets with port 53               |
| `tcp.srcport == 1234` | Show all TCP packets originating from port 1234 | `udp.srcport == 1234` | Show all UDP packets originating from port 1234 |
| `tcp.dstport == 80`   | Show all TCP packets sent to port 80            | `udp.dstport == 5353` | Show all UDP packets sent to port 5353          |

## Application Level Protocol Filters | HTTP and DNS
|                                 |                                                |                           |                          |
| ------------------------------- | ---------------------------------------------- | ------------------------- | ------------------------ |
| Filter                          | Description                                    | Filter                    | Description              |
| `http`                          | Show all HTTP packets                          | `dns`                     | Show all DNS packets     |
| `http.response.code == 200`     | Show all packets with HTTP response code "200" | `dns.flags.response == 0` | Show all DNS requests    |
| `http.request.method == "GET"`  | Show all HTTP GET requests                     | `dns.flags.response == 1` | Show all DNS responses   |
| `http.request.method == "POST"` | Show all HTTP POST requests                    | `dns.qry.type == 1`       | Show all DNS "A" records |
## Filter: "contains"

|                 |                                                                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Filter          | **contains**                                                                                                                                 |
| **Type**        | Comparison Operator                                                                                                                          |
| **Description** | Search a value inside packets. It is case-sensitive and provides similar functionality to the "Find" option by focusing on a specific field. |
| **Example**     | Find all "Apache" servers.                                                                                                                   |
| **Workflow**    | List all HTTP packets where packets' "server" field contains the "Apache" keyword.                                                           |
| **Usage**       | `http.server contains "Apache"`                                                                                                              |

## Filter: "matches"

|             |                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| Filter      | **matches**                                                                                                   |
| Type        | Comparison Operator                                                                                           |
| Description | Search a pattern of a regular expression. It is case insensitive, and complex queries have a margin of error. |
| **Example** | Find all .php and .html pages.                                                                                |
| Workflow    | List all  HTTP packets where packets' "host" fields match keywords ".php" or ".html".                         |
| **Usage**   | `http.host matches "\.(php\|html)"`                                                                           |
## Filter: "in"

|             |                                                                                |
| ----------- | ------------------------------------------------------------------------------ |
| Filter      | **in**                                                                         |
| Type        | Set Membership                                                                 |
| Description | Search a value or field inside of a specific scope/range.                      |
| Example     | Find all packets that use ports 80, 443 or 8080.                               |
| Workflow    | List all TCP packets where packets' "port" fields have values 80, 443 or 8080. |
| Usage       | `tcp.port in {80 443 8080}`                                                    |

## Filter: "upper"

|             |                                                                                                             |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| Filter      | **upper**                                                                                                   |
| Type        | Function                                                                                                    |
| Description | Convert a string value to uppercase.                                                                        |
| Example     | Find all "APACHE" servers.                                                                                  |
| Workflow    | Convert all  HTTP packets' "server" fields to uppercase and list packets that contain the "APACHE" keyword. |
| Usage       | `upper(http.server) contains "APACHE"`                                                                      |

## Filter: "lower"

|             |                                                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------------------------------- |
| Filter      | **lower**                                                                                                        |
| Type        | Function                                                                                                         |
| Description | Convert a string value to lowercase.                                                                             |
| Example     | Find all "apache" servers.                                                                                       |
| Workflow    | Convert all  HTTP packets' "server" fields info to lowercase and list packets that contain the "apache" keyword. |
| **Usage**   | `lower(http.server) contains "apache"`                                                                           |
## Filter: "string"

|             |                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------- |
| Filter      | **string**                                                                               |
| Type        | Function                                                                                 |
| Description | Convert a non-string value to a string.                                                  |
| Example     | Find all frames with odd numbers.                                                        |
| Workflow    | Convert all "frame number" fields to string values, and list frames end with odd values. |
| Usage       | `string(frame.number) matches "[13579]$"`                                                |

## Nmap Scans

Nmap is an industry-standard tool for mapping networks, identifying live hosts and discovering the services. As it is one of the most used network scanner tools, a security analyst should identify the network patterns created with it. This section will cover identifying the most common Nmap scan types.

- TCP connect scans
- SYN scans
- UDP scans

It is essential to know how Nmap scans work to spot scan activity on the network. However, it is impossible to understand the scan details without using the correct filters. Below are the base filters to probe Nmap scan behaviour on the network. 

**TCP flags in a nutshell:**

|   |   |
|---|---|
|**Notes**|**Wireshark Filters**|
|Global search.|- `tcp`<br><br>- `udp`|
|- Only SYN flag.<br>- SYN flag is set. The rest of the bits are not important.|- `tcp.flags == 2`<br><br>- `tcp.flags.syn == 1`|
|- Only ACK flag.<br>- ACK flag is set. The rest of the bits are not important.|- `tcp.flags == 16`<br><br>- `tcp.flags.ack == 1`|
|- Only SYN, ACK flags.<br>- SYN and ACK are set. The rest of the bits are not important.|- `tcp.flags == 18`<br><br>- `(tcp.flags.syn == 1) and (tcp.flags.ack == 1)`|
|- Only RST flag.<br>- RST flag is set. The rest of the bits are not important.|- `tcp.flags == 4`<br><br>- `tcp.flags.reset == 1`|
|- Only RST, ACK flags.<br>- RST and ACK are set. The rest of the bits are not important.|- `tcp.flags == 20`<br><br>- `(tcp.flags.reset == 1) and (tcp.flags.ack == 1)`|
|- Only FIN flag<br>- FIN flag is set. The rest of the bits are not important.|- `tcp.flags == 1`<br><br>- `tcp.flags.fin == 1`|