## High level approach
The file itself can be considered as a bridge, and contains multiple ports inside it.
When the program start, bridge is initialized with a given bridge id, and several ports given by port number.
When a packet is received by a port, if it is a BPDU message, it will be checked globally and inside each port 
if there is a new root bridge. When data is detected, it will process the data by first querying the forward table to 
see if it is in there. If so, then forward the message to the given port, otherwise, broadcast the message to 
all ports.

##Challenge
The largest challenge we face is how to build the minimum spanning tree. We first have no idea about how to record the
best BPDU among all message that is heard. Another challenge is to find the port that should be closed when there are multiple
bridge linked to it. We realized these two challenges can be solved by adding a bpdu_list at both globally and locally in 
each port to check if it is the best bpdu it has ever heard to determine the root bridg and the designated port.

##Design
The BPDU list is a very useful design to determine the spanning tree, bpdu class is also a class that can ease many workload
since the comparison can be done by using python's sort method. Also, we implemented a sent_data list to avoid data to be send multiple times.

##Testing
We added multiple print statements to help us debug. We also check each config manually to see if the designated port, 
root port, and disabled port are the one that should be. Also, we checked the spanning tree by changing the packets to 1 to see
if the spanning tree is correctly formed.