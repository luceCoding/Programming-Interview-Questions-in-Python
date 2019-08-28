# Block Storage
- Data are chopped up into blocks of bytes, leaving the system unknown as to what it is storing until all the blocks are put back together.
- Data are able to be retrieved at low latency. Very high performance. However, this means that storage cannot have a lot of distance between each other, generally multiple servers will be in the same physical location from each other.
- Cannot deal with many users editing the same file. No locking ability among data.
- No metadata. Very little over head.
- NAS index tables have a max size, limited to the amount of data to be indexed or stored due to performance hit. Therefore, scalability is limited.
- Requires backup to an offsite location for redundancy.

# File Storage
- Stores data in a file hierarchy or known as directories and sub-directories.Similar to how linux/UNIX systems organize their files.
- Has a set uncustomizable metadata for each file, things like file name, creation date, file type etc...
- Allows many users to edit the same data. Has locking features, however, is handled by the operating system and not by the file system itself.
- Designed to be on a local network or remote network. Flexible on location, however, impacting latency further the servers are. Performance is not a concern.
- NAS index tables have a max size, limited to the amount of data to be indexed or stored due to performance hit. Therefore, scalability is limited.
- Requires backup to an offsite location for redundancy.

# Object Storage
- Stores data in objects, each object has an unique ID, metadata and the actually data itself. Each object is then stored into buckets or a group of objects, the user can decide which bucket each object can be placed in.
- Meant to organize unstructured data, whether that is videos, music, documents, or pictures into a flat organiaation with flexibe sized buckets.
- Limited to the number of users allowed the edit the same file at a time. If many users do edit the same file, object storage will instead create different versions of that object.
- Buckets can be stored in multiple nodes and geographic locations. This creates builtin redundancy and improves performance.
- Allows custom metadata, this allows the filtering or processing in finding the correct data by saving what category each data is in its metadata. For example, YouTube would use some sort of object storage to find cat videos versus dog videos by using the metadata for each object.
- Since Object Storage uses a GUID for each object, we can scale the number of objects easily instead of relying on NAS which uses complex file paths to determine where data are.

## Resources
- [Block vs. File vs. Object Storage Video](https://www.youtube.com/watch?v=qTGDhvbdPzo)
