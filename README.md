# iptables
A script for managing IPtables configurations.

This is a simple script that I've been using for many years to configure
iptables.  There are many alternatives but this is what I use.

# Install

```sh
cp iptables /etc/init.d/
cp default /etc/default/iptables
```

On Debian based systems activate the script with:

```sh
sudo update-rc.d iptables defaults 13 87
```

# Configuration

You should only need to edit ``/etc/default/iptables``.  The configuration
is controlled mainly by setting variables.

## Variables

### EXTIF

This variable sets the external interface.  The default is ``eth0`` but it
could be something else.  Currently only one external interface is supported.

### MASQIFS

A space separated list of interfaces to be masqueraded.  This will masquerade the ``EXTIF`` to these interfaces.  You can also masquerade
interfaces using the ``masquerade`` inside of the ``start_local`` function.

Masquerading allows the masqueraded interface to access the Internet via
the ``EXTIF`` but does not open any ports on the masqueraded interface.

### MODULES

A space separated list of kernel modules to load.  Any modules which are
needed by the iptables configuration should be listed here.

### OPENTCP

A space separated list of TCP ports that should be open.

### OPENUDP

A space separated list of UDP ports that should be open.

### OPENIPS

A space separated list of IPv4 and IPv6 addresses that should be allow in on
all ports.

### BLOCKEDIPS

A space separated list of IPv4 and IPv6 addresses that should be completely
blocked.

### PING

Set to ``true`` to enable remote IPv4 and IPv6 ping.

### LOG

Set to ``true`` to enable iptables IPv4 and IPv6 logging.

### VERBOSE

Set to ``true`` to enable verbose output.

### IPTABLES

Set to the path to the ``iptables`` executable.  Default is
``/sbin/iptables``.

### IP6TABLES

Set to the path to the ``ip6tables`` executable.  Default is
``/sbin/ip6tables``.  Set to ``/bin/true`` to make the script ignore IPv6.

## Functions

The script defines a few helper functions.

### start_local

A special function that is called after the initial iptables setup.  You
can define this function to implement more advanced configurations.  From
this function you can call the other helper functions listed below.

### action <message> <command> [args...]

This will print the specified message to the screen when the ``iptables``
scripts is run followed by a message indicating if the the command was
successful or failed.  Use this to log the action being performed.

### allow_in <protocol> <port> <source>

Allows connections to the specified port on the specified protocol from the
specified source IPv4 or IPv6 address or address range.

### block_ip <address>

Blocks all input from an IPv4 or IPv6 address.

### allow_ip <address>

Allows all input from an IPv4 or IPv6 address.

### allow_port <protocol> <port>

Allows input from a port and protocol.

### masquerade <internal> <external>

Masquerades the internal network interface to the specified external
interface.

# Usage

You can run the script manually and see its output like this:

```sh
sudo /etc/init.d/iptables restart
```

Otherwise, use your systems service control commands, in which case
the scripts output will not be visible on the command line.  E.g.:

```sh
sudo systemctl restart iptables
```