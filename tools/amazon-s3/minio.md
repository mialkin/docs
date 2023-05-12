# MinIO

[↑ MinIO](https://min.io) is an object storage solution that provides an Amazon Web Services S3-compatible API and supports all core S3 features.

MinIO is built to deploy anywhere — public or private cloud, bare metal infrastructure, orchestrated environments, and edge infrastructure.

MinIO's [↑ documentation](https://min.io/docs/minio).

## Table of contents

- [MinIO](#minio)
  - [Table of contents](#table-of-contents)
  - [Buckets](#buckets)
  - [Users](#users)
  - [Policies](#policies)

## Buckets

MinIO uses buckets to organize objects. A bucket is similar to a folder or directory in a filesystem, where each bucket can hold an arbitrary number of objects.

**Versioning** allows to keep multiple versions of the same object under the same key.

**Object Locking** prevents objects from being deleted. Required to support retention and legal hold. Can only be enabled at bucket creation.

**Quota** limits the amount of data in the bucket.

**Retention** imposes rules to prevent object deletion for a period of time. Versioning must be enabled in order to set bucket retention policies.

## Users

A MinIO user consists of a unique access key (username) and corresponding secret key (password). Clients must authenticate their identity by specifying both a valid access key (username) and the corresponding secret key (password) of an existing MinIO user.
Groups provide a simplified method for managing shared permissions among users with common access patterns and workloads.

Users inherit access permissions to data and resources through the groups they belong to.
MinIO uses Policy-Based Access Control (PBAC) to define the authorized actions and resources to which an authenticated user has access. Each policy describes one or more actions and conditions that outline the permissions of a user or group of users.

Each user can access only those resources and operations which are explicitly granted by the built-in role. MinIO denies access to any other resource or action by default.

## Policies

MinIO uses Policy-Based Access Control (PBAC) to define the authorized actions and resources to which an authenticated user has access. Each policy describes one or more actions and conditions that outline the permissions of a user or group of users.

MinIO PBAC is built for compatibility with AWS IAM policy syntax, structure, and behavior.

Each user can access only those resources and operations which are explicitly granted by the built-in role. MinIO denies access to any other resource or action by default.
