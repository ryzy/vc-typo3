# TYPO3 Neos with Vagrant

Complete environment for TYPO3 Neos using Vagrant + Chef + Berkshelf provisioning.

##### Features:
* CentOS 6 based
* Chef + Berkshelf provisioning
* MySQL 5.5 / Nginx / PHP 5.5 installed && configured
* phpMyAdmin
* TYPO3 Neos installed

## Requirements

1. Install [Vagrant](http://www.vagrantup.com/)
2. Install [VirtualBox](https://www.virtualbox.org/)
3. Make sure you use Ruby 1.9.x or 2.x.
  Note: you might follow this article on [how to install different version of Ruby using RVM](http://misheska.com/blog/2013/06/16/using-rvm-to-manage-multiple-versions-of-ruby/)).
4. Make sure you have Ruby Bundler installed:
  ```[sudo] gem install bundler```

## Usage

First you need to install required gems (specified in Gemfile) and vagrant plugins:

```bash
bundle install
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-berkshelf
vagrant plugin install vagrant-omnibus
```

Later on simply use:
```bash
vagrant up
```

**Go to [192.168.66.6](http://192.168.66.6/)** to see VM's default vhost. You'll see there phpinfo() and link to phpMyAdmin (user:root, password:password)

Add `neos.local` to your `hosts` file:
```bash
192.168.66.6 neos.local
```

**Go to [neos.local](http://neos.local/)** to see TYPO3 Neos page (or [neos.local/setup](http://neos.local/setup) to kick off installation process).

Mount `/var/www` to your host filesystem:
```
sudo mount_nfs -o resvport 192.168.66.6:/var/www /Volumes/vc-typo3-var-www
```

## Tips & Tricks

##### Mount VM's `/var/www` to your filesystem

This VM is configured to export `/var/www` via NFS so it's possible to mount it to host's filesystem - not the other way, like when using Vagrant's `synced_folder` directive. **It gives you much better performance**, even if you'd use `type:'nfs'` option in your Vagrantfile.

On my setup the difference was ca. 300%: for `ab -n 100 -c 3 http://neos.local/` the performance for uncached request (FLOW_CONTEXT=Development) I had 3 req/s vs. 1req/s when using Vagrant's `synced_folder` with `type:'nfs'`.

You might use more tuned version of `mount_nfs`:
```bash
mount_nfs -o async,udp,vers=3,resvport,intr,rsize=32768,wsize=32768,soft 192.168.66.6:/var/www /Volumes/vc-typo3-var-www
```


## Author

Author:: ryzy (<marcin@ryzycki.com>)
