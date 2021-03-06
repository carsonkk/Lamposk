# Lamposk
A C implementation of a distributed ticketing kiosk system based on [Lamport's Distributed Solution](http://lamport.azurewebsites.net/pubs/time-clocks.pdf)

## About
#### Assumptions
- Using TCP sockets, one kiosk can only talk to one client at a time, thus up to X independent client-datacenter transactions can occur at once, where X is the number of datacenters
- Used getopt.h for command line parsing, libconfig.h for configuration file parsing, and a slew of networking libraries for system communication

## Project Summary
### Generals
- 1 central ticket pool
- 5 datacenters (kiosks)
- 100 tickets
- X number of clients
- Use lamport's distributed model for mutual exclusion
- Can't have negative unit count
- Buying is all or nothing; if not enough tickets are available to fulfill the order, it fails

### Client
##### Implementation
- Send request to a datacenter with num of tickets
- Receive reply from same datacenter with fulfillment flag
##### UI
- Ask which datacenter to connect to (IP, tag, etc, read from .cfg)
- ask how many tickets to buy, which triggers the delayed request
- display the request result received from the datacenter

### Datacenter
##### Implementation
- Sync with other datacenters over number of tickets/who holds access to ticket pool
- Receive request from clients for tickets
- Coordinate with other datacenters over ticket pool access
- Broadcast release message with number of tickets sold
- Reply back to client regarding request fulfillment
##### UI
- Log all information about transactions to the console including message passing, unit count, clock value
- 5 second delay when sending a message
- TCP primatives
##### Notes
- Each datacenter stores its own view of the unit count
- maintian a logical lamport clock <clk, proc_id> for breaking ties