# rbs
reverse interactive command server over ssh

## features

* reverse command/shell using ssh from local/remote server to another remote server
* no ssh tunnel needed (no TCPForwarding/Getway needed)
* just need ssh access to remote to serve a local shell/command on it
* no listennig process on shell served server, only use standard ssh connection
* optionnal password protected connection (using gpg)
* serve interactive shell between 2 servers using a middle server

## schema

start reverse shell serve from local `host` on `remote` (need ssh access to `<remote>` from `host`) and connect to shell from `<remote>`:

```
host$ rbs -r <remote>
<local> bash -> ssh <remote>
connect to <host> shell from <remote>:
remote$ rbs
access host shell
```

## usage

* Bypass any totally useless/buggy security tools protection auditing your tty, like CyberArk CAC, that is wrongly injecting bad Ctl-C to your tty, as they are totally unable to parse the command you are executing.
* Foolish security guys are just preventing you to work properly and don't understand how they are themselves dangerous for system security (breaks/truncates files during vim save/breaks interactive batch during run/scramble tmux sessions...).
* rbs allow you to have a full functionnal interactive session to a corrupted server by a stupid security tool, skipping all auditing/tty survey, this is a POC, not to be used in real life (even if rbs stays really discrete, and won't be detected).
* Advice: Never use it in your enterprise
