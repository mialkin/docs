# Amazon S3

[↑ Amazon S3](https://aws.amazon.com/s3) or **Amazon Simple Storage Service** is a service offered by Amazon Web Services, AWS, that provides object storage through a web service interface.

The basic storage units of Amazon S3 are *objects* which are organized into *buckets*. Each object is identified by a unique, user-assigned key. Buckets can be managed using the console provided by Amazon S3, programmatically with the AWS SDK, or the REST application programming interface. Requests are authorized using an [↑ access control list](https://en.wikipedia.org/wiki/Access-control_list) associated with each object bucket and support versioning which is disabled by default. Since buckets are typically the size of an entire file system mount in other systems, this access control scheme is very coarse-grained. In other words, unique access controls cannot be associated with individual files.

## File size limits

An object in S3 can be between 0 bytes and 5 TB. If an object is larger than 5 TB, it must be divided into chunks prior to uploading. When uploading, Amazon S3 allows a maximum of 5 GB in a single upload operation; hence, objects larger than 5 GB must be uploaded via the S3 multipart upload API.
