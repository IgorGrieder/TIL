# Streams

I'm writing this article because recently I came across in a .NET application a big problem that we sometimes commit: using a lot of memory. To give more context, I was investigating why an Azure Function was constantly restarting the k8s pod in production, causing a massive delay in the processing of some key financial data for the application users. After seeing the logs I saw that the process was being killed because of memory usage, exiting with code 137 Out Of Memory (OOM), with a lot of memory spikes before exiting.

When I was investigating the function, that is responsible for producing excel files I saw a major flaw: an excel sheet was being created and was being filled using an MemoryStream and instead of using this stream to upload the file in a storage it was actually using the .ToArray() and then was loading this hole array of bytes into the blob later on. Now imagine, if just an small amount of files, let's say 10 files of 10mb were being processed in parallel, this implementation would cause an overhead of 200mb of program memory usage.

To fix this not optimal implementation my solution was simple, I was going to handle the data transfer using Streams!

## What's a Stream

A Stream is an abstraction for processing loads of data in batches, instead of loading all the bytes into memory at once. To understand how it actually works we can think about the following situation. Imagine we are in a restaurant sitting in a table with 20 people and we all asked to drink coke cola. The waiter or waitress can deliver all of those cans at once but it would be very hard, since it's tray fits only 5 cans at a time (this would be our memory limitations in the environment). So the waitress decides to serve the table 4 times, given it would be better in this situation, even thought he or she would have more effort going back and forth (this would represent our flushing process in each stream read batch).

## Linkup

This metaphor actually explain in a simple way what my implementation was doing. I had a big excel file produced and instead of using an MemoryStream and then transforming it into an array of bytes I just created a write blob stream and wrote my data directly into it. This simple change eliminated my memory problem and my Azure Function was operating fine after it.

## Conclusion

Sometimes we tend not to think about memory in general, but in controlled environments it is always crucial to understand what would happen when scale hits. If you have anything to comment about it or you would like to share ideas about the topic leave a comment!
