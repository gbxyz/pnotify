# NAME

pnotify - A simple, portable Perl script for sending DNS NOTIFY packets
with TSIG support.

# SYNOPSIS

pnotfy \[OPTIONS\] \[ZONE\]

Options:

    --zone=ZONE             The DNS zone (can also be passed as a bare
                            argument)
    --class=CLASS           The class (default IN)
    --server=HOST           Host to send the packet to, OR
    --dsync[=TYPE]          Use DSYNC (RFC 9859) to determine the
                            destination. Specify a value to limit the
                            notifications to TYPE (either CDS or CSYNC).
    --srv=DOMAIN            FQDN to use to construct _dns._udp SRV query
                            to determine which servers to send the
                            NOTIFY to
    --port=PORT             Destination port to send the packet to
                            (default 53, ignored if --srv or --dsync are
                            used)
    --timeout=TIMEOUT       Timeout in seconds (default 1)
    --tsig-name=NAME        Optional TSIG name
    --tsig-key=KEY          Optional TSIG KEY
    --tsig-algorithm=NAME   Optional TSIG algorithm (default HMAC-MD5)
    --source=ADDR           Use this source address (optional)
    --report-channel=DOMAIN Specify RFC 9567 report channel (optional)
    --help                  Show this help

# SENDING MULTIPLE PACKETS

You can send a NOTIFY to a single host using the --server argument. If
you provide a value for the --srv argument, then pnotify will perform
a SRV lookup and retrieve a list of servers, and send a packet to each.

For example: if you used `--srv=example.com`, pnotify will perform a SRV
query for `_dns._udp.example.com`. A NOTIFY packet will then be sent to
each host in the response to the SRV query.

## DSYNC support

pnotify supports DSYNC ([RFC 9859](https://www.rfc-editor.org/rfc/rfc9859.html)).
If you pass the --dsync argument, it will look for the DSYNC endpoint(s)
published by the parent operator and extract the target(s) and port(s)
found. You can limit the endpoints that messages are sent to be
providing a value, either `CDS` or `CSYNC`.

# OUTPUT

pnotify will print the rcode of the response from each server or
"TIMEOUT" to STDOUT.

# EXIT CODE

The exit code will be equal to the number of errors observed.

# RATIONALE

It is sometimes useful to be able to manually send a NOTIFY packet to a
DNS server. There are other tools to do this (eg nsd-notify(8)) but they
are not as portable as pnotify, which is a pure Perl script with only a
limited range of prerequisites.

# REQUIREMENTS

- [Net::DNS](https://metacpan.org/pod/Net%3A%3ADNS)
- [Getopt::Long](https://metacpan.org/pod/Getopt%3A%3ALong)
- [Pod::Usage](https://metacpan.org/pod/Pod%3A%3AUsage)

# COPYRIGHT

Copyright 2011-2023 CentralNic Ltd, 2023-2026 Gavin Brown. This program
is Free Software, you can use it and/or modify it under the same terms
as Perl itself.

# SEE ALSO

- [Net::DNS](https://metacpan.org/pod/Net%3A%3ADNS)
