# vagrant vboxlxd
Vagrant is cool, but spinning up ephemeral, throwaway VMs with it is kinda tedious. lxd is cool, but on macOS, it's not easily available if you want to run it all locally. This will spin up a modest Ubuntu VM, initialize LXD on a separate virtual disk with ZFS, apply an opinionated config, and cache some lxc images to make things more responsive.

If you want something a bit more permanent than this, you'll want to [natively run an lxd client](https://insights.ubuntu.com/2017/02/27/lxd-client-on-windows-and-macos/) and a separate LXD host.

## Getting it running

### Prereqs
1. For now, you need Virtualbox and Vagrant. It's best if you have vagrant 1.8+, as this tries to make use of the new `linked_clone` support.
2. It's highly recommended to install the `vagrant-vbguest` plugin:```vagrant plugin install vagrant-vbguest```
3. If you have a proxy hanging around, you might want to install [vagrant-vbguest](https://github.com/tmatilai/vagrant-proxyconf) to help speed up repetitive package installs, depending on how awesome your ISP is.

Just a ```vagrant up``` and ```vagrant ssh``` is all you need to get running.


## Handy scripts added:
in the scripts directory, you'll find some quick scripts.

script | description
------ | ------------
`ephem` | gives you an ephemeral container that lives until you exit it. Great when you need a throwaway container for a simple, single task.
`nuke_all_lxc` | Quickly-ish forcefully deletes all running containers. 
