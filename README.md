# rbs
reverse interactive command server over ssh

## usage

* Bypass any totally useless/buggy security tools protection auditing your tty, like CyberArk CAC, that is wrongly injecting bad Ctl-C to your tty, as they are totally unable to parse the command you are executing.
* Foolish security guys are just preventing you to work properly and don't understand how they are themselves dangerous for system security (breaks/truncates files during vim save/breaks interactive batch during run...).
* rbs allow you to have a full functionnal interactive session to a corrupted server by a dangerous security tool, skipping all auditing/tty survey, this is a POC, not to be used in real life.
* Advice: Never use in this in your enterperise
