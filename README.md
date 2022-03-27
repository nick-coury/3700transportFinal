# 3700_bridge

## Approach

1. Requirements   

We were asked to write the code to emulate a BGP Router. BGP routers are similar to network bridges in the way that they both have forwarding tables and they receive and send messages. One big difference between routers and bridges is that routers need to aggregate their forwarding table. 

- Accept route update messages from the BGP neighbors, and forward updates as appropriate
- Accept route revocation messages from the BGP neighbors, and forward revocations as appropriate
- Forward data packets towards their correct destination
- Return error messages in cases where a data packet cannot be delivered
- Coalesce forwarding table entries for networks that are adjacent and on the same port
- Serialize your routing table cache so that it can be checked for correctness

2. High Level Code Structure 

``` python


```

3. Implementation 

At a high level, here was our approach: 

- Separate functionality for each message type
    - Handshake message: 
        - When our router starts up, it needs to send a handshake messages to each of its neighbor routers.
    - Update message:
        - These messages tell your router how to forward data packets to a specific destination on the internet.
        - Aggregate entries in the forward table.
    - Withdraw message:
        - These messages tell your router that a route in its forward table is no longer valid.
        - Remove the entry from the forward table and reaggregate. 
    - Data message:
        - These messages get routed to their destination according to the forward table.
        - Data messages have no impact on the router, they are simple messages that get routed. 
    - Dump and Table messages:   
        - These messages are useful for testing and is a way to output your entire forward table. 
        - The convention is to recieve a dump message and to respond with a table message.   



## Challenges 

Our biggest challenge in this assignment was catching dropped packets. We found this particularly hard because it made us question our implementation of selective acks. 

## Program Features

The program is well documented: every choice is commented with an explanation of what is going on and the data structures/variable types are explicitly stated. Additionally, our code has a couple helper methods that enable more testing such as flushing our buffer. 

Our code is also well abstracted making further development easier. In the case that any message type is changed or new types are added, our code has a framework to support it.


## Testing 

Testing first began with writing code to create a stop and wait protocol. 

Once our protocol correctly stoped and waited, we handled duplicated packets. We tested that duplicates were not printed out by ensuring that each message had sequence numbers.   

Once we could handle duplicated messages, we implemented basic support for out of order messages. We verified that messages were sent in order by reading through the program logs and manually tracking the message output. We also implemented a buffer on the receiver end with an accompanying flush method for further testing.

After verifying that our receiver could receive out of order messages we focused on dropped packets. This was by far the hardest step to test, but we managed to verify that dropped packets were detected by adding an abstracted layer of ack messages. We tested the new level of ack messages (acks for acks) with recycled testing logic for stop and wait ack messages.

Being able to detect and rememdy dropped packets allowed us to focus on corrupted packets. We used a standard library (zlib) to implement data checksum's. We then packed the data checksum into the message to be sent and had the receiver verify the checksum. 

To test various latencies and bandwidths we primarily relied on the built in testing methods, but we also added many log statements to make sure each step of our code ran as intended. 





