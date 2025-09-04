# Streams

I'm writing this short article because recently I came across a big problem that we sometimes make: using a lot of memory.

To give more context, I was investigating why an Azure Function was constantly restarting the k8s pod in production, causing a massive delay in the processing of some key financial data for the application users. After seeing the logs I saw that the process was being killed because of memory usage, exiting with code 137 Out Of Memory (OOM), with a lot of memory spikes before exiting.

There was a significant inefficiency in the file-producing function. It created an Excel sheet, wrote it's content to a MemoryStream, and then, instead of using the stream directly to upload the content, it was converted to a byte array. This entire byte array was then passed to some functions and then in the upstream layer, to load into the blob, it created another memoryStream based on the data and just after that the file was uploaded. This is a major flaw that unnecessarily consumes memory and processing power.

Now imagine, if just an small amount of files, let's say 10 files of 10mb were being processed in parallel, this implementation would cause an overhead of 200mb of program memory usage for some times, even stressing the GC.

### Bad code choice

```C#
var someMemoryStream = new MemoryStream();
var worbook = new XSSFWorkbook();

// After fiiling the workbook

workbook.Write(someMemoryStream);
byte[] data = someMemoryStream.ToArray()

///... after some lines of code in other layers

// A MemoryStream is created from the original data
await using (var otherMemoryStream = new MemoryStream(data))
{
    await blobClient.UploadAsync(otherMemoryStream);
}
```

To fix this not optimal implementation my solution was simple, I was going to handle the data transfer using a blob storage Stream directly!

```C#
var worbook = new XSSFWorkbook();

// After filling the workbook just write it to the opened stream
await using (var blobStream = await blobClient.OpenWriteAsync(true))
{
    workbook.Write(blobStream);
}
```

## What's a Stream

A Stream is an abstraction for processing loads of data in batches, instead of loading all the bytes into memory at once.

To understand how it actually works we can think about the following situation. Imagine we are in a restaurant sitting in a table with 20 people and we all asked to drink coke cola. The waiter or waitress can deliver all of those cans at once but it would be very hard, since it's tray fits only 5 cans at a time (this would be our memory limitations in the environment). So the waitress decides to serve the table 4 times, given it would be better in this situation, even thought he or she would have more effort going back and forth (this would represent our flushing process in each stream read batch).

Streams in programming are widelly used an we can see how efficient they are in a simple example, such as the one I gave in this article or even in the foundations of the Event Driven Paradigm, being present in the heart of Kafka for example.

![Stream image of flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xopuv2q2q55g57zvah3l.png)

## Linkup to the problem

This metaphor actually explains in a simple way what my implementation was doing. I had a big excel file produced and instead of using an MemoryStream and then transforming it into an array of bytes I just created a write blob stream and wrote my data directly into it. This simple change eliminated my memory problem and my Azure Function was operating fine after it.

## Conclusion

Sometimes we tend not to think about memory when working in languages with a higher level of abstraction, but in controlled environments it is always crucial to understand what would happen when scale hits. If you have anything to comment or you would like to share ideas about the topic leave a comment below!
