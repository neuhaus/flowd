# flowd

This is flowd, a NetFlow collector daemon intended to be small, fast and
secure. It works well with hardware flow probes (e.g. routers) softflowd[1],
pfflowd[2], and other software agents that export NetFlow v.1, v.5, v.7 or 
v.9 datagrams.

It features some basic filtering to limit or tag the flows that are
recorded and is privilege separated, to limit security exposure from
bugs in flowd itself. Flowd is IPv6 capable - supporting flow export via
IPv6 transport and NetFlow v.9 IPv6 flow records. It also supports reception
of flow datagrams sent to multicast groups, allowing one to build redundant 
flow gathering systems.

flowd does not try to do anything beyond accepting NetFlow packets and 
writing them to disk. In particular, it does not do any analysis and it
doesn't support storage into SQL databases. These tasks are left (in 
typical Unix fashion) to separate programs. Some example tools (including 
one to store flows in a SQL database) are provided in the `tools/` directory.

At present, flowd is considered stable enough for production deployment. 
Some more features are planned before the 1.0 release (see the `TODO` file 
if you want to help), but everything that is documented should be working 
now. Please report any problems to djm@mindrot.org. Bugs may also be reported
[using the Bugzilla](http://bugzilla.mindrot.org/).

flowd stores records on disk in a compact binary format, see store.h
for a specification in the form of a C header file. Perl, Python and C
APIs are provided to managing the log files that flowd creates. Example 
applications are `flowd-reader.c`, `reader.pl` and `reader.py`. More useful 
applications live in the `tools/` directory, please refer to the 
`tools/README.tools` file for an explanation of what they are. These example
apps will require that the relevant Perl/Python modules are installed as 
described in the `INSTALL` document.

This on-disk format is a parametised format capable of storing a
superset of NetFlow v.5, including the most common records from NetFlow
v.9. Exactly which components of the NetFlow records actually get
written to disk may be specified at runtime, so the logs can be made
quite compact by excluding information that is uninteresting to you.
An optional, per-record CRC32 checksum is provided to detect log
corruption. Efforts are made to ensure that flows are written atomically
to disk, and backed out when a write fails.

At present, flowd supports NetFlow v.1, v.5, v.7 and v.9 packet formats
over both IPv4 and IPv6 transports. Future plans include sflow and IETF
IPFIX protocol support (when it is finalised). See the `TODO` file in
this distribution for more information (and more interesting projects if
you are a prospective developer)

flowd is tested on OpenBSD and Linux. It may work on other platforms,
but will likely need some adjusting. Please refer to the `PLATFORMS` file
for detailed notes on platform support and testing.

Large parts of this code have been shamelessly taken from OpenBSD, in
particular bgpd (the configuration parser) and OpenSSH (the privsep
fd passing and CRC32 code), sudo and libc. All of this code is under
BSD-like licenses, but read the `LICENSE` file for details.

Damien Miller <djm@mindrot.org>

## Project description from google code

### small, fast and secure NetFlow collector

flowd is a small, fast and secure NetFlow™ collector. It offers the following features:

 * Understands NetFlow protocol v.1, v.5, v.7 and v.9 (including IPv6 flows)
 * Supports both IPv4 and IPv6 transport of flows
 * Secure: flowd is privilege separated to limit the impact of any compromise
 * Supports filtering and tagging of flows, using a packet filter-like syntax
 * Stores recorded flow data in a compact binary format which supports run-time choice over which flow fields are stored
 * Ships with both Perl and Python interfaces for reading and parsing the on-disk record format
 * Is licensed under a liberal BSD-like license
 * Supports reception of flow export datagrams sent to multicast groups (IPv4 and IPv6), thereby allowing the construction of redundant flow collector systems

flowd works with any standard NetFlow exporter, including hardware devices (e.g. routers) or software flow tracking agents, such as my own softflowd and pfflowd. Please refer to the README for more information.

The flowd sensor follows the Unix philosophy of "doing one thing well" - it doesn't try to do anything beyond accepting NetFlow packets and storing them in a standard format on disk. In particular, it does not include support for storing flows in multiple formats or performing data analysis. That sort of thing is left to external tools. The source distribution includes several example tools including a basic reporting script and one to store flows in a SQL database.
