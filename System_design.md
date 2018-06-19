# CAP Theroem
This was created by Eric Brewer in the 2000s to help model distrubuted system design. 
His idea was that a system can provide three basic needs to a user, consistency, avaliability and partition tolerance.
However, a system can only optimized two of the three while the third would have to be relaxed or ignored completely.

### Consistency
A system operates fully or not at all.
Every read receives the most recent write or an error.

### Avaliability 
A system is always able to answer a request.
Every request recieves a non-error response.

### Partition Tolerance
If a node or more fails, the system can continue to function.
The system continues to operate despite an arbitrary number of messages being dropped by the network between nodes.

# ACID vs BASE
### ACID
- Atomicity
- Consistency
- Isolation
- Durability

For example, an SQL server may follow the ACID properities. 
Banks usually use SQL servers and therefore follow the ACID properities.
If there was a tranasction and a user's account balance isn't updated, any requests for that balance would be denied or postponed until that update completes.

### BASE
- Basically Avaliable
- Soft State
- Evenutal Consistency

If you were twitter, BASE would be a good model for you. You don't necessarily need everyone's news feed to be up to date all the time.
But eventually you want everyone's news feed to be up to date but you can sacrifice consistency for other advantages.
