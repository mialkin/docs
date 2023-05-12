# Amazon S3

[↑ Amazon S3](https://aws.amazon.com/s3) or **Amazon Simple Storage Service** is a service offered by Amazon Web Services, AWS, that provides object storage through a web service interface.

The basic storage units of Amazon S3 are *objects* which are organized into *buckets*. Each object is identified by a unique, user-assigned key. Buckets can be managed using the console provided by Amazon S3, programmatically with the AWS SDK, or the REST application programming interface. Requests are authorized using an [↑ access control list](https://en.wikipedia.org/wiki/Access-control_list) associated with each object bucket and support versioning which is disabled by default. Since buckets are typically the size of an entire file system mount in other systems, this access control scheme is very coarse-grained. In other words, unique access controls cannot be associated with individual files.

## File size limits

An object in S3 can be between 0 bytes and 5 TB. If an object is larger than 5 TB, it must be divided into chunks prior to uploading. When uploading, Amazon S3 allows a maximum of 5 GB in a single upload operation; hence, objects larger than 5 GB must be uploaded via the S3 multipart upload API.

## Multipart upload

Multipart upload allows you to upload a single object as a set of parts. Each part is a contiguous portion of the object's data. You can upload these object parts independently and in any order. If transmission of any part fails, you can retransmit that part without affecting other parts. After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object. In general, when your object size reaches 100 MB, you should consider using multipart uploads instead of uploading the object in a single operation.

Using multipart upload provides the following advantages:

- **Improved throughput** – You can upload parts in parallel to improve throughput.
- **Quick recovery from any network issues** – Smaller part size minimizes the impact of restarting a failed upload due to a network error.
- **Pause and resume object uploads** – You can upload object parts over time. After you initiate a multipart upload, there is no expiry; you must explicitly complete or stop the multipart upload.
- **Begin an upload before you know the final object size** – You can upload an object as you are creating it.

We recommend that you use multipart upload in the following ways:

- If you're uploading large objects over a stable high-bandwidth network, use multipart upload to maximize the use of your available bandwidth by uploading object parts in parallel for multi-threaded performance.
- If you're uploading over a spotty network, use multipart upload to increase resiliency to network errors by avoiding upload restarts. When using multipart upload, you need to retry uploading only the parts that are interrupted during the upload. You don't need to restart uploading your object from the beginning.

[↑ Multipart upload process](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html#mpu-process).

## Downloading an object

[↑ `GetObject`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)
