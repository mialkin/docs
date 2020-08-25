# scp

Secure copy (remote file copy program); scp copies files between hosts on a network.  It uses ssh for data transfer, and uses the same authentication and provides the same security as ssh.  scp will ask for passwords or passphrases if they are needed for authentication.

## local → remote

```sh
scp /file/to/send username@remote:/where/to/put
```

## remote → local

Copy from remote to local:

```sh
scp username@remote:/file/to/send /where/to/put
```
