```
sudo lxc cluster list
sudo lxc storage create local dir --target <node>
(need to run this command for each node of the microcloud)
sudo lxc storage create local dir
sudo lxc profile device add default root disk path=/ pool=local
```
