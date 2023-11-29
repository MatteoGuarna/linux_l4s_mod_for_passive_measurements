# Linux L4S kernel patch for delay bit

This is a clone of the linux kernel tree with l4s patch available at [L4S Team](https://github.com/L4STeam/linux).

This branch contains the modifications required to implement the delay bit EFM technique as described in the [IETF RFC 9506](https://datatracker.ietf.org/doc/rfc9506/). 

In order to ensure that the L4S Dual Queue (dualpi2) works correctly, the routers' interface where dualpi2 is deployed need not only to have dualpi2 enabled with traffic control, but also tbf with the latency parameter explicited (any value should work). This is not specified in the parent project but is in fact necessary to ensure that Dual Queue is not starving the Prague flows.
```bash
sudo tc qdisc add dev $INPUT_IF root handle 1: tbf rate 1000mbps latency 10ms burst 1540
sudo tc qdisc add dev $INPUT_IF parent 1: dualpi2
#the modifications must be added to the router's input as well as output interface
sudo tc qdisc add dev $OUTPUT_IF root handle 1: tbf rate 1000mbps latency 10ms burst 1540
sudo tc qdisc add dev $OUTPUT_IF parent 1: dualpi2
```

