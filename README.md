[![Joknarf Tools](https://img.shields.io/badge/Joknarf%20Tools-Visit-darkgreen?logo=github)](https://joknarf.github.io/joknarf-tools)
[![Shell](https://img.shields.io/badge/shell-bash%20-blue.svg)]()


# rbs
reverse interactive command server over ssh, and reverse ssh proxy

## features

* reverse command/shell using ssh from local/remote server to another remote server
* reverse ssh proxy server
* no ssh tunnel needed (no TCPForwarding/Getway needed)
* just need ssh access to remote to serve a local shell/command on it, using simple fifo to communicate
* no listening process on shell served server, only use standard ssh connection
* optionnal password protected connection (using gpg)
* serve interactive shell between 2 servers using a middle server
* use unix socket or ip port to serve on remote server to connect
* caveeat: automatic resize of tty size by SIGWINCH signal won't occur in session (need to use for example xterm `/bin/resize`)

## prerequites

* `socat` on `remote` server for `rbs`
* `gpg` (optionnal password authent)
* xterm resize (optional to have tty resized to your terminal size)

* `nc` on `local` + `remote` for `rbsprox`
  
## reverse shell/command serve/connect

![rbs2](https://github.com/user-attachments/assets/54172be2-8c89-4684-8d8e-33af74f1e11e)

start reverse shell serve from local `host` on `remote` (need ssh access to `<remote>` from `host`) and connect to shell from `<remote>`:

```
start reverse shell on host to listen on <remote>:
local$ rbs -r <remote>
<local> spawn interactive bash -> ssh <remote> -> listen <sock>

connect to <host> shell from <remote>:
remote$ rbs
<remote> -> <sock> -> <host> -> interactive bash
access host shell
```
add `Include ~/.ssh/rbs` in your ~/.ssh/config to avoid having `rbs` to use `-F ~/.ssh/rbs` ssh option

process view on `host` exposing the shell:
```
► 2674779 (joknarf) [bash] 20:05 bash
  ├─2674784 (joknarf) [script] 20:05 script -qfc exec bash --rcfile .bashrc /dev/null
  │ └─2674786 (joknarf) [bash] 20:05 bash -l
  └─2674785 (joknarf) [ssh] 20:05 ssh remote
```

if force use ssh (-s) for local host (uses `ssh local` instead of `script` command):
```
► 2676448 (joknarf) [bash] 20:09 bash
  ├─2676453 (joknarf) [ssh] 20:09 ssh local
  └─2676454 (joknarf) [ssh] 20:09 ssh remote
```

## reverse ssh proxy

start a reverse ssh proxy server:
`<local>` connects to `<remote>` to initiate unixsocket, then `<remote>` can use `<local>` as proxy without ssh connection to it.
```
local$ rbs -r <remote> -x
<local> -> <spawn nc on demand to any host> -> ssh <remote> -> listen <sock>

# connect ssh to any host from <remote> using proxy on <local> (ssh -> unix socet to <local> -> <anyhost>:22)
remote$ ssh -F ~/.ssh/rbs_proxy <anyhost>
<remote> -> ssh <anyhost> -> proxy command use <sock> to contact <local> -> <local> spawn nc to <anyhost> -> <anyhost>
```

using `rbs` only 1 connection at a time is possible, to have full featured reverse ssh proxy use `rbsprox`
```
local$ rbsprox -r <remote>
<local> -> ssh - <remote> <wait for proxy demand>
<local> receive demand -> nc <-> ssh remote

# connect ssh to multiple hosts from <remote> using proxy on <local>
remote$ rbsprox <anyhost>
<remote> -> ask proxy for <anyhost> -> nc -> <local> -> <anyhost>
or use ssh -F ~/.ssh/rbs_proxy <anyhost>
``` 

## usage

* Bypass any totally useless/buggy security tools protection auditing your tty, like "CyberFart" CAC, that is wrongly injecting bad Ctl-C to your tty, as they are totally unable to parse the command you are executing.
* Foolish security guys are just preventing you to work properly and don't understand how they are themselves dangerous for system security (breaks/truncates files during vim save/breaks interactive batch during run/scramble tmux sessions...).
* rbs allow you to have a full functionnal interactive session to a corrupted server by a stupid security tool, skipping all auditing/tty survey, this is a POC, not to be used in real life (even if rbs stays really discrete, and won't be detected).
* Advice: Never use it in your enterprise to bypass security
