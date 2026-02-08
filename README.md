# rbs
reverse interactive command server over ssh

## features

* reverse command/shell using ssh frm local/remote server to remote server
* no ssh tunnel needed (no TCPForwarding/Getway needed)
* optionnal password protected connection

## schema

start reverse shell on host (need ssh access to `<remote>`) and connect to shell from `<remote>`:

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
* rbs allow you to have a full functionnal interactive session to a corrupted server by a stupid security tool, skipping all auditing/tty survey, this is a POC, not to be used in real life.
* Advice: Never use in this in your enterperise
