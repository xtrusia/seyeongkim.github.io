---
layout: post
title: Build devstack based openstack
nav_order: 2
reflowMarkdown.preferredLineLength: 60
---

I tried devstack 2023.1

https://docs.openstack.org/devstack/latest/

git checkout -b 2023.1 origin/stable/2023.1


local.conf

{% highlight shell %}
[[local|localrc]]
TARGET_BRANCH=stable/2023.1
ADMIN_PASSWORD=123qwe
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD

enable_service neutron-dns
Q_ML2_PLUGIN_EXT_DRIVERS=port_security,qos,dns
IP_VERSION=4
ENABLE_DEBUG_LOG_LEVEL=True
SYSLOG=True
VERBOSE=True
LOG_COLOR=True
LOGDAYS=3
GLANCE_LIMIT_IMAGE_SIZE_TOTAL=5000
DOWNLOAD_DEFAULT_IMAGES=False
disable_service tempest
disable_service dstat
disable_service memory_tracker

{% endhighlight %}

./stack.sh

# expand quota

# image over 1G
{% highlight shell %}
stack@devstack:/etc/keystone$ cat policy.yaml
identity:update_registered_limit: rule:admin_required
identity:delete_registered_limit: rule:admin_required
identity:create_registered_limits: rule:admin_required


systemctl restart devstack@key*


openstack registered limit delete 57539850f0a7425abe43f306eb80a369
openstack registered limit create --service glance --default-limit 5000 --region RegionOne image_size_total


{% endhighlight %}

https://github.com/openstack/devstack/blob/master/doc/source/configuration.rst#glance
