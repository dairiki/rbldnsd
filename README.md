Enhanced IPv6 support for rbldnsd
=================================

News
----

### 2013-06-29

This work has now been incorporated upstream and [released as rbldnsd
0.997](http://www.corpit.ru/pipermail/rbldnsd/2013q2/001169.html).  A
source [tarball](http://www.corpit.ru/mjt/rbldnsd/rbldnsd-0.997.tar.gz) can
be downloaded from the the [main rbldnsd
page](http://www.corpit.ru/mjt/rbldnsd.html).


### 2013-03-12

After some experimentation, I’ve completely re-implemented the trie
code.  The new code uses much less memory than the original code (by a
factor of three or so.)  I’ve blown away the old contents of my
[ipv6](https://github.com/dairiki/rbldnsd/tree/ipv6) branch and
non-fast-forwarded to the new version.  (Sorry for any pain that
causes; the old code is still available at the
[abandoned/ipv6_1](https://github.com/dairiki/rbldnsd/tree/abandoned/ipv6_1)
tag.

Where’s the code?
-----------------

The code is in the [ipv6](https://github.com/dairiki/rbldnsd/tree/ipv6) branch.
(There is nothing in the ``master`` branch except for this github README (what you’re reading now).)

You can browse the code [here](https://github.com/dairiki/rbldnsd/tree/ipv6), you can grab a tarball [here](http://github.com/dairiki/rbldnsd/tarball/ipv6/rbldnsd.tar.gz), or you can checkout the code using [git](http://git-scm.com/) by doing something like

```
git clone -b ipv6 git://github.com/dairiki/rbldnsd.git
```

What’s here
-----------

### An ip6trie dataset

This adds an ``ip6trie`` dataset type to
[rbldnsd](http://www.corpit.ru/mjt/rbldnsd.html).

The ``ipv6`` dataset is just like the existing ``ip4trie``, except it
is for IPv6 addresses.
It can handle CIDR ranges of any prefix length;
each CIDR range can have its own (A and TXT) values.
CIDR ranges can also be excluded.

Here’s a sample data file, which shows the sorts of things it supports.

```
# example data for ip4trie dataset
$NS 1w ns1.ex.com ns2.ex.com
$SOA 1w ns1.ex.com admin.ex.com 0 2h 2h 1w 1h
# default data
:2:Sample data ($)
dead:beef:ea7s:b0f0
f007:ba11:1200/40
1234:5678:abcd:effa:4200:1972::/112 :4:Different data
# An exclusion
!dead:beef:ea7s:b0f0::10
# A larger exclusion
!dead:beef:ea7s:b0f0::1:0/124
```

### Improved memory usage for the ip4trie dataset

Plugging my new trie implementation into the the ``ip4trie`` dataset
has reduced its memory utilization by roughly a factor of three.
Now the ``ip4trie`` dataset takes only 50% more memory than the ``ip4set``
dataset to store a zone of 64k randomly generated 32-bit IP4 addresses.

### IP6 ACL support

IP6 addresses are now allowed (alongside IP4 addresses) in the ``acl`` dataset.


Installation
------------

Same as for stock rbldnsd:

```
./configure
make
make install
```

Testing
-------

The new trie code does some hairy tricks with structure packing in
order to save memory.  As of now, I’ve only tested the code on Intel
machines.  *In particular this code “should work” but has not been
tested in a big-endian compilation environment.*  If you have success
(or trouble) on a big-endian system, please send me a report.

If you encounter problems, there are some tests for the new trie
implementation.  To run those, try:

```
./configure
make
make check
```

Also, internal debugging assertions in the trie code may be enabled by
specifying the ``--enable-asserts`` configure option:

```
./configure --enable-asserts
make
...
```
