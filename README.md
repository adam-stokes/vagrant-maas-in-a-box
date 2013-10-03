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
* Needs [vagrant][2] 1.3.3
* Needs [ansible][3] installed on host machine

# Install

```
% git clone git://github.com:battlemidget/vagrant-maas.git
% cd vagrant-maas
% ./import-boxes
% ./start
```

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

