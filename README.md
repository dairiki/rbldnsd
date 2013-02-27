Enhanced IPv6 support for rbldnsd
=================================

What’s here
-----------

This adds an ``ip6trie`` dataset type to [rbldnsd](http://www.corpit.ru/mjt/rbldnsd.html).

The ``ipv6`` dataset is just like the existing ``ip4trie``, except it is for IPv6 addresses.  It can handle CIDR ranges of any prefix length; each CIDR range can have its own (A and TXT) values.  CIDR ranges can also be excluded.

Here’s a sample data file, which shows the sorts of things it supports.

```
# example data for ip4trie dataset
$NS 1w ns1.ex.com ns2.ex.com
$SOA 1w ns1.ex.com admin.ex.com 0 2h 2h 1w 1h
# default data
:2:Sample data ($)
dead:beef:ea7s:b0f0
f007:ba11:1200/40
1234:5678:abcd:effa:4200:1972:0:0/112 :4:Different data
# An exclusion
!dead:beef:ea7s:b0f0:0:0:0:10
# A larger exclusion
!dead:beef:ea7s:b0f0:0:0:1:0/124
```

Where’s the code?
-----------------

The code is in the [ipv6](https://github.com/dairiki/rbldnsd/tree/ipv6) branch.  (There is nothing in the ``master`` branch except for this github README (what you’re reading now).)

You can browse the code [here](https://github.com/dairiki/rbldnsd/tree/ipv6), you can grab a tarball [here](http://github.com/dairiki/rbldnsd/tarball/ipv6/rbldnsd.tar.gz), or you can checkout the code using [git](http://git-scm.com/) by doing something like

```
git clone -b ipv6 git://github.com/dairiki/rbldnsd.git
```

IPv6 ACL support
----------------

Work is also in progress on adding IPv6 support to the ``acl`` dataset.
That work is currently not very tested, if you're interested, look
in the [wip](https://github.com/dairiki/rbldnsd/tree/wip) branch.


Installation
------------

Same as for stock rbldnsd:

```
./configure
make
make install
```

