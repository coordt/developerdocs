# SSH Configuration

Add this bit of text to your `~/.ssh/config` file. It will allow you to access the test server by its name. For example `ssh coolorg-test`.

```
Host bastion
Hostname 11.11.11.16
User coolorgdev
IdentityFile ~/.ssh/id_rsa

Host coolorg-test
Hostname 10.10.10.104
User coolorgdev
ProxyCommand ssh -A -T bastion nc %h %p
```
