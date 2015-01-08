---
layout: post
title: "Don't copy your Vagrantfile"
date: 2015-01-08
comments: true
categories: vagrant dry vagrantfile ruby
---

## Problem

You're using `Vagrant` and you copy `Vagrantfile` from older project to new one.
Then you always need to edit it to accommodate the requirements of a new project.

## Solution: Ruby & YAML

Because `Vagrantfile` is just `Ruby` code you can simplify your process and have
all Vagrant related code in one repository.

### Create a Vagrant project

Here you want to include some `Ansible`, `Chef`, `Docker`, `Puppet`, ...
recipes to script your environment. Then, create a `Vagrantfile`.

### Example Vagrantfile

{% highlight ruby %}
require 'yaml'

# Every project will have different server_config.yml file
CONF = YAML.load_file('server_config.yml')

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  CONF['forward_ports'].each do |port|
    config.vm.network :forwarded_port, guest: port['guest'], host: port['host']
  end

  CONF['shared_dires'].each do |dir|
    config.vm.synced_folder dir['guest'], dir['host'], type: 'nfs'
  end

  config.vm.network :private_network, ip: CONF['private_ip']
  config.vm.network :public_network,  bridge: 'en0: Wi-Fi (AirPort)'

  config.vm.provider :virtualbox do |v|
    v.customize ['modifyvm', :id, '--name',   CONF['name']]
    v.customize ['modifyvm', :id, '--memory', CONF['memory']]
  end

  # config.vm.provision :ansible do |ansible|
  #   ansible.playbook = 'setup.yml'
  #   ansible.inventory_path = 'hosts'
  #   ansible.limit = 'all'
  # end
end
{% endhighlight %}

### Example server_config.yml

{% highlight yaml %}
name: project_name
memory: 512
private_ip: 192.168.50.4
forward_ports:
  - { guest: 3000, host: 3000 }
shared_dires:
  - { guest: '~/development/project_name', host: '/home/vagrant/project_name' }
{% endhighlight %}


## What goes into new projects

To reuse previous `Vagrantfile` and have specific setting per project,
you'll need to add another (almost dummy) `Vagrantfile` to the new
project with one line of ruby code.

{% highlight ruby %}
#!/usr/bin/ruby

eval(File.open("#{Dir.home}/path/to/vagrant/project/Vagrantfile").read)
{% endhighlight %}

Also create new `server_config.yml` file inside the new project.

Now you can run `vagrant up` and other vagrant commands from new project.

## This is not the end

With time, your project environment requirements will grow.
Break up configuration to smaller parts or files, use Ruby modules, `bash` or
anything to automate and simplify your environment.
