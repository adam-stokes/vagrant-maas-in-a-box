# vagrant-maas-in-a-box

### MAAS in a box, with a fox, eating his socks.

Quickly get up and running with [MAAS][1]. Maybe you are a developer
wanting to test latest MAAS bits or you are presenting MAAS to a
potential client?

# What you get

1 MAAS server, 1-99 Nodes

## In the pipeline:

- Add some juju love
- Add nodes from different virtualization platforms.

# Pre-reqs

* Needs virtualbox `% sudo apt-get install virtualbox`
* Needs [vagrant][2] 1.3.3 until [bug #2307][4] can be fixed.
* Needs [ansible][3] installed on host machine

# Setup

Disable ansible's ssh host checking, edit `/etc/ansible/ansible.cfg` and alter the following

```
[ssh_connection]
# ssh arguments to use
# Leaving off ControlPersist will result in poor performance, so use.
# paramiko on older platforms rather than removing it
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no
```

# Install
```
% git clone git://github.com:battlemidget/vagrant-maas.git
% cd vagrant-maas
% ./import-boxes
% ./start
```

# How long does this take?

* Running a time gives me about: not long >:)
* System: Intel(R) Core(TM) i7-2600 CPU @ 3.40GHz, 8 cpu's, 12G ram

# Maas IP setup

On your maas instance enable port forwarding.

```
echo 1 > /proc/sys/net/ipv4/ip_forward
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```

# Accessing

In your browser visit `http://localhost:8080/MAAS`

Default login and password: `admin` `pass`

# Do you Juju

```
sudo add-apt-repository ppa:juju/stable
sudo apt-get update && sudo apt-get install juju-core

```

## Edit environments.yaml

Copy your $APIKEY(MAAS keys) from `http://localhost:8080/MAAS/account/prefs/` and replace that in the `maas-oauth` section

```
maas:
  type: maas
  maas-server: 'http://localhost:8080/MAAS'
  maas-oauth: '$APIKEY'
  admin-secret: 'nothing'
  default-series: 'precise'
  # Generating key pair
  # % ssh-keygen
  # Follow prompts, then `cat <key.pub>` and append this to `authorized-keys`
  authorized-keys: ssh-rsa <ssh-rsa string>
```

## Boostrap

`sudo juju bootstrap`

# Environment Variables

* MAAS_NUM_NODES = Sets how many nodes you wish to activate, valid range is 1-99
* MAAS_NODE_GUI  = Starts your instances with VirtualBox gui enabled.

# Author

Adam Stokes <adam.stokes@ubuntu.com>

# LICENSE

Apache 2.0

 [1]: http://maas.ubuntu.com
 [2]: http://vagrantup.com
 [3]: http://ansibleworks.com/docs/gettingstarted.html#ubuntu-and-debian
 [4]: https://github.com/mitchellh/vagrant/issues/2307

