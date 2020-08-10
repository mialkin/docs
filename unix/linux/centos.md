# CentOS

Check info about OS:

```sh
hostnamectl
```

Check OS name and version:

```sh
cat /etc/*-release
```

Check kernel version:

```sh
uname -a
```

## Checking for and updating packages

To see which installed packages on your system have updates available, use the following command:

To update all packages and their dependencies, enter yum update (without any arguments):

```sh
yum check-update
```

## Updating All Packages and Their Dependencies

```sh
yum update
```

## See also

[YUM](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/ch-yum)
