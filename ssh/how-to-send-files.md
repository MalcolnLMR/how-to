# How to send files via ssh

## SCP

scp [options] [source] [destination]

### Options you can use

* -P - Specifies the remote host ssh port.
* -p - Preserves file modification and access times.
* -q - Use this option if you want to suppress the progress meter and non-error messages.
* -C - This option forces scp to compress the data as it is sent to the destination machine.
* -r - This option tells scp to copy directories recursively.

### Example

Copy files all files at Documents folder, from remote server to local at port 5000, in to current directory

```bash
scp -r -P 5000 user@0.0.0.0:~/Documents .
```

Send README.md file from current directory to remote server at Downloads

```bash
scp README.md user@0.0.0.0:~/Downloads
```


