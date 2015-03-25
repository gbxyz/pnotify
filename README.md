# NAME

pnotify - A simple, portable Perl script for sending DNS NOTIFY packets
with TSIG support.

# SYNOPSIS

pnotfy \[options\]

Options:

        --zone=ZONE             The DNS zone
        --class=CLASS           The class (default IN)
        --server=HOST           Host to send the packet to, OR
        --srv=DOMAIN            FQDN to use to construct _dns._udp SRV query
                                to determine which servers to send the 
                                NOTIFY to
        --port=PORT             Destination port to send the packet to (default 53)
        --timeout=TIMEOUT       Timeout in seconds (default 1)
        --tsig-name=NAME        Optional TSIG name
        --tsig-key=KEY          Optional TSIG KEY (only HMAC-MD5 is
                                supported)
        --source=ADDR           Use this source address (optional)
        --help                  Show this help

# SENDING MULTIPLE PACKETS

You can send a NOTIFY to a single host using the --server argument. If you
provide a value for the --server argumnet, then pnotify will perform a SRV
lookup and retrieve a list of servers, and send a packet to each.

For example: if you used `--srv=example.com`, pnotify will perform a SRV
query for `_dns._udp.example.com`. A NOTIFY packet will then be sent to
each host in the response to the SRV query.

# OUTPUT

pnotify will print the rcode of the response from each server or "TIMEOUT"
to STDOUT.

# EXIT CODE

The exit code will be equal to the number of errors observed.

# RATIONALE

It is sometimes useful to be able to manually send a NOTIFY packet to a
DNS server. There are other tools to do this (eg nsd-notify(8)) but they
are not as portable as pnotify, which is a pure Perl script with only a
limited range of prerequisites.

# REQUIREMENTS

- [Net::DNS](https://metacpan.org/pod/Net::DNS)
- [Getopt::Long](https://metacpan.org/pod/Getopt::Long)
- [Pod::Usage](https://metacpan.org/pod/Pod::Usage)

# COPYRIGHT

Copyright 2011 CentralNic Ltd. This program is Free Software, you can
use it and/or modify it under the same terms as Perl itself.

# SEE ALSO

- [Net::DNS](https://metacpan.org/pod/Net::DNS)
- [https://www.centralnic.com/](https://www.centralnic.com/)
