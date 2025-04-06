# Streams

In node.js we have a stream interface that handles streams of data. We can think
of streams as like a water pipe that non stop handles water flow. This same
concept can be used to node.js streams, they're an abstraction to handle flows
of data.

## Usage

The usage of stream can be, for an example, in an AWS S3 integration on an API.
While getting the images in the storage AWS returns a stream of data in a chunk
format. This way is considered better then storing all the file in the RAM and
then sending it to the backend since we can while processing the file already
send it to the front end with the `.pipe()` method.

### Code example

```JavaScript
collectionRoutes.get('/image/:imageId', async (req, res) => {
  // Get the S3 stream
  const imageData = await s3.getS3ObjectStream(imageId);

  // Set HTTP headers for the browser
  res.setHeader('Content-Type', imageData.contentType);

  // Connect the S3 stream to the HTTP response
  imageData.stream.pipe(res);
});
```

## Not using the pipe

In some cases we actually need to load the whole file into memory so all the
stream data from AWS has to be awaited and then we group it in an buffer

```JavaScript
// Convert stream to buffer
const chunks = [];
for await (const chunk of imageData.stream) {
  chunks.push(chunk);
}
const buffer = Buffer.concat(chunks);
```

### Buffer

In this situation a Buffer is used instead of a regular array since it is a data
type that handles Binary data better. It stores in the V8 memory (C++), not in the
standard JavaScript Heap and has some array specific transformations.
