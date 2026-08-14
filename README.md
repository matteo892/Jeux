Tools:
  --help            / -h  : Shows all tools options.
  --version         / -v  : Displays the version and information of the tool.
  --settings-update / -su : Update the tools settings.
  * --mode          / -m  : Mode: decorated / interface
  * --status        / -s  : Status: enable / disable

Pentesting:
  --advanced-scanner      / -as  : Advanced scanning performing all scans. (website, domain, IP, server)
  * --target              / -t   : Service target: <URL> / <domain> / <IP[:port]> / <localhost[:port]>
    --output              / -o   : Creating additional JSON output.
    --http-timeout        / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --socket-timeout      / -ST  : Set the maximum socket timeout in seconds: <timeout>
    --http-proxy          / -HP  : Set an HTTP proxy: <proxy:port>
    --socket-proxy        / -SP  : Set a socket proxy: <proxy:port>
    --useragent           / -u   : Set a user-agent: random / <useragent>
    --cookie              / -c   : Set a cookie: <cookie>
  --vulnerability-scanner / -vs  : Scan all vulnerabilities of a website.
  * --target              / -t   : Website target: <URL> / <domain> / <IP:port> / <localhost:port>
    --output              / -o   : Creating additional JSON output.
    --http-timeout        / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --http-proxy          / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent           / -u   : Set a user-agent: random / <useragent>
    --cookie              / -c   : Set a cookie: <cookie>
  --port-scanner          / -ps  : Scan the ports of an IP.
  * --target              / -t   : IP target: <IP>
  * --mode                / -m   : Scan mode: single / multiple / range / default / all
    --port                / -p   : Port(s): single: <port> / multiple: <port>,<port> / range: <port>-<port>
    --protocol-scan       / -PS  : Protocol(s): TCP / UDP / TCP,UDP
    --output              / -o   : Creating additional JSON output.
    --socket-timeout      / -ST  : Set the maximum socket timeout in seconds: <timeout>
    --socket-proxy        / -SP  : Set a socket proxy: <proxy:port>
  --url-discovery-crawler / -udc : Scan all urls of a website.
  * --target              / -t   : Website target: <URL> / <domain> / <IP:port> / <localhost:port>
  * --mode                / -m   : Scan mode: onlypage / allwebsite
    --output              / -o   : Creating additional JSON output.
    --http-timeout        / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --http-proxy          / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent           / -u   : Set a user-agent: random / <useragent>
    --cookie              / -c   : Set a cookie: <cookie>
  --ip-pinger             / -ip  : Continuously ping an IP.
  * --target              / -t   : IP target: <IP>
  * --mode                / -m   : Ping mode: ICMP / TCP
    --bytes               / -b   : Set the number of bytes for an ICMP ping: <bytes>
    --port                / -p   : Set the port for a TCP ping: <port>
    --interval            / -i   : Set the interval between each ping in seconds: <interval>
    --socket-timeout      / -ST  : Set the maximum socket timeout in seconds: <timeout>
    --socket-proxy        / -SP  : Set a socket proxy: <proxy:port>
  --host-discovery        / -hd  : Determines which hosts are online.
  * --target              / -t   : CIDR target: <IP>/<CIDR prefix>
    --port                / -p   : Set the port for a TCP ping: <port>
    --output              / -o   : Creating additional JSON output.
    --tcp-icmp-timeout    / -TIT : Set the maximum TCP/ICMP timeout in seconds: <timeout>
    --socket-proxy        / -SP  : Set a socket proxy: <proxy:port>

Osint:
  --dorking-query-engine     / -dqe : Query builder for Google, Bing and DuckDuckGo with advanced operators.
  * --engine                 / -e   : Search engine: google / bing / duckduckgo
  --wallet-tracker           / -wt  : Track a crypto wallet's transactions with APIs.
  * --address                / -a   : Wallet target address: <address>
    --output                 / -o   : Creating additional JSON output.
    --http-timeout           / -HT  : Set the maximum HTTP timeout for the API in seconds: <timeout>
    --http-proxy             / -HP  : Set an HTTP proxy for the API: <proxy:port>
    --useragent              / -u   : Set a user-agent for the API: random / <useragent>
  --username-tracker         / -ut  : Track a username across multiple platforms.
  * --target                 / -t   : The target username: <username>
    --output                 / -o   : Creating additional JSON output.
    --http-timeout           / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --http-proxy             / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent              / -u   : Set a user-agent: random / <useragent>
  --email-tracker            / -et  : track an email registered on several platforms.
  * --email                  / -e   : Email target: <email>
    --output                 / -o   : Creating additional JSON output.
    --http-timeout           / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --http-proxy             / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent              / -u   : Set a user-agent: random / <useragent>
  --email-lookup             / -el  : Retrieve public data from an email.
  * --email                  / -e   : Email target: <email>
    --output                 / -o   : Creating additional JSON output.
    --socket-timeout         / -ST  : Set the maximum socket timeout in seconds: <timeout>
    --socket-proxy           / -SP  : Set a socket proxy: <proxy:port>
  --ip-lookup                / -il  : Fetch public IP data using the "ip-api.com" API.
  * --ip                     / -i   : IP target: <IP>
    --output                 / -o   : Creating additional JSON output.
    --http-timeout           / -HT  : Set the maximum HTTP timeout for the API in seconds: <timeout>
    --http-proxy             / -HP  : Set an HTTP proxy for the API: <proxy:port>
    --useragent              / -u   : Set a user-agent for the API: random / <useragent>
  --phone-number-lookup      / -pnl : Retrieve public data from a phone number.
  * --phone                  / -p   : Phone number target: <number>
    --output                 / -o   : Creating additional JSON output.
  --instagram-profile-lookup / -ipl : Retrieve public data from an instagram username.
  * --target                 / -t   : Username target: <username>
  * --sessionid              / -s   : Your instagram id session: <sessionid>
    --output                 / -o   : Creating additional JSON output.
    --http-proxy             / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent              / -u   : Set a user-agent: random / <useragent>

Utilities:
  --file-metadata-scanner / -fms : Scan all file metadata.
  * --path                / -p   : The file path: <path>
    --output              / -o   : Creating additional JSON output.
  --file-metadata-deleter / -fmd : Remove all file metadata.
  * --path                / -p   : The file path: <path>
  --website-cloner        / -wc  : Clone the entire web page.
  * --target              / -t   : Website target: <URL> / <domain> / <IP:port> / <localhost:port>
    --http-timeout        / -HT  : Set the maximum HTTP timeout in seconds: <timeout>
    --http-proxy          / -HP  : Set an HTTP proxy: <proxy:port>
    --useragent           / -u   : Set a user-agent: random / <useragent>
    --cookie              / -c   : Set a cookie: <cookie>

Notations:
  /  : Or
  [] : Optional
  <> : Value
  *  : Required
