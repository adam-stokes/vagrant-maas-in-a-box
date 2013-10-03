# vagrant-maas-in-a-box

### MAAS in a box, with a fox, eating his socks.

Quickly get up and running with [MAAS][1]. Maybe you are a developer
wanting to test latest MAAS bits or you are presenting MAAS to a
potential client?

# What you get

1 MAAS server, 10 Nodes

## In the pipeline:
- Add some juju love
- Add nodes from different virtualization platforms.

# Pre-reqs

* Needs virtualbox `% sudo apt-get install virtualbox`
* Needs [vagrant][2] 1.3.3 until [bug #2307][4] can be fixed.
* Needs [ansible][3] installed on host machine

# Install

Disable ansible's ssh host checking, edit `/etc/ansible/ansible.cfg` and alter the following

```
[ssh_connection]
# ssh arguments to use
# Leaving off ControlPersist will result in poor performance, so use.
# paramiko on older platforms rather than removing it
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no
```

```
% git clone git://github.com:battlemidget/vagrant-maas.git
% cd vagrant-maas
% ./import-boxes
% ./start
```

# How long does this take?

* Running a time gives me about: not long >:)
* System: Intel(R) Core(TM) i7-2600 CPU @ 3.40GHz, 8 cpu's, 12G ram

# Provisioning

The previous installer step should've run ansible after image was
loaded. If not just run the below command

```
% vagrant provision maas
% vagrant provision nodeX
```

# Accessing

In your browser visit `http://localhost:8080/MAAS`

Default login and password: `admin` `pass`

# Author

Adam Stokes <adam.stokes@ubuntu.com>

# LICENSE

Apache 2.0

 [1]: http://maas.ubuntu.com
 [2]: http://vagrantup.com
 [3]: http://ansibleworks.com/docs/gettingstarted.html#ubuntu-and-debian
 [4]: https://github.com/mitchellh/vagrant/issues/2307

