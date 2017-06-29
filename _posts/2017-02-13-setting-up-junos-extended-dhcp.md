---
layout: post
title:  "Configuring Junos Extended DHCP"
date:   2017-02-13 23:30:00
categories: juniper technical_tips
---
As of 13.2 (for most platforms), Juniper ethernet switches now offer Enhanced Layer 2 Services (ELS) which changed
the syntax to configure a number of different objects throughout Junos. Recently I had
to configure DHCP on an EX switch for a customer on EX3300s which were running ELS.

Though Juniper has [documentation](https://www.juniper.net/documentation/en_US/junos12.3/topics/task/configuration/subscriber-management-address-assignment-pools-configuring-overview.html) describing how to configure DHCP on ELS I found that it was light on the details particularly around the situation I needed to support where I had non-contiguous address blocks within the
same network address space.

In my case the customer was running a /23 network block but had static configurations on some PCs out of that address
space so the EX was going to allocate from one small block and then a larger one. If this is on different networks it's possible to configure [overflow pools](https://www.juniper.net/documentation/en_US/junos12.3/topics/task/configuration/subscriber-management-address-assignment-pool-linking.html) and link them together.

The key pieces of information that I found missing was:
<ol>
  <li>You must have a network address on the network for which you want to provide DHCP. Note that if you don't have a local address there is no warning on commit, there's just no DHCP handed out.</li>
  <li>Multiple address ranges configured in the same pool will overflow from one to the other as needed.</li>
</ol>

In the end our configuration came out looking like the following:

{% highlight java %}
system {
    services {
        dhcp-local-server {
            group LAN {
                interface irb.1
            }
        }
    }
}
interfaces {
    irb {
        unit 1 {
            family inet {
                address 1.2.3.17/23;
            }
        }
    }
}
access {
    address-assignment {
        pool LAN {
            family inet {
                network 1.2.2.0/23;
                range 1st-range {
                    low 1.2.2.200;
                    high 1.2.2.254;
                }
                range 2nd-range {
                    low 1.2.3.3;
                    high 1.2.3.16;
                }
                dhcp-attributes {
                    name-server {
                        8.8.8.8
                    }
                    router {
                        1.2.2.1;
                    }
                }
            }
        }
    }
}
{% endhighlight %}
or
{% highlight java %}
set system services dhcp-local-server group LAN
set access address-assignment pool LAN family inet network 1.2.2.0/23
set access address-assignment pool LAN family inet range 1st-range low 1.2.2.200
set access address-assignment pool LAN family inet range 1st-range high 1.2.2.254
set access address-assignment pool LAN family inet range 2nd-range low 1.2.3.3
set access address-assignment pool LAN family inet range 2nd-range high 1.2.3.16
set access address-assignment pool LAN family inet dhcp-attributes name-server 8.8.8.8
set access address-assignment pool LAN family inet dhcp-attributes router 1.2.4.1
set interfaces irb unit 1 family inet address 1.2.3.17/23
{% endhighlight %}
