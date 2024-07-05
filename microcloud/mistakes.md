# My mistakes

- re-initialization of MicroCloud takes a little bit of effort and sometimes time.
- While reinitializing make sure each snap has been removed properly on all nodes.
- During bringing up the microcloud one of the lan connections to the raspberry pi got disconnected (reboot fixed this).
- after reinstalling snaps, the daemon of the services (eg: snap.lxd.daemon or snap.microcloud.daemon or snap.<program>.daemon)
  is stopped and not running these need to be physically enabled (sudo systemctl enable --now snap.<program>.daemon)
- Before running `microcloud init` make sure you reboot all the nodes once.

# Computer generated mistakes

- removal of lxd was halted and caused issues. (This was fixed by physically stopping snap.lxd.daemon via systemctl and then rebooting)
