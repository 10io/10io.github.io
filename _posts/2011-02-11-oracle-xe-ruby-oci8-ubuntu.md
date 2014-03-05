---
layout: post
title: Quick guide installing Oracle XE + ruby-oci8 on Ubuntu 10.04
tags: ubuntu ruby
---

Since installing [Oracle XE][1] (stands for eXpress Edition, basically a free edition) on [Ubuntu 10.04 _Lucid Lynx_][2] can be a little hell(read you can spend several hours banging your head against the wall of your choice), I'm writing this quick guide for reference. Bonus for Ruby enthusiasts: we will also install [ruby-oci8][3]. Let's start.

## Update the system

It's a good habit to update the system before installing anything:
{% highlight bash %}
$ sudo aptitude update
$ sudo aptitude safe-upgrade
{% endhighlight %}

## Install Oracle XE

First install two needed libs:

```bash
$ sudo aptitude install libaio1 libaio-dev bc
```

To install Oracle XE you need to add a new repository. Add this line at the end of /etc/apt/sources.list

```text
deb http://oss.oracle.com/debian unstable main non-free
```

Now, update the system:

```bash
$ sudo aptitude update
```

If you have an error saying that there is a problem with the repository key, do the following instead:

```bash
$ wget http://oss.oracle.com/el4/RPM-GPG-KEY-oracle  -O- | sudo apt-key add -
$ sudo aptitude update
```

Now, install Oracle XE:

```bash
$ sudo aptitude install oracle-xe-universal oracle-xe-client
```

## Configuring Oracle XE
As easy as

```bash
$ /etc/init.d/oracle-xe configure
```

it will ask a password for users SYS and SYSTEM

You can now verify that Oracle XE it's up and running, browsing:

```text
http://localhost:8080/apex/
```

## Bonus: installing ruby-oci8
Before anything, you have to set up some environment variables. At the end of your ~/.bashrc file, append:

```bash
export PATH=$PATH:/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/bin
ORACLE_HOME=/usr/lib/oracle/xe/app/oracle/product/10.2.0/server
export ORACLE_HOME
export ORACLE_SID=XE
LD_LIBRARY_PATH=/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/lib
export LD_LIBRARY_PATH
```

*Restart* your terminal.

You're good to go:

```bash
$ sudo gem install ruby-oci8
```

If an error occurs about Oracle initialization, there is a good chance that your environment variables are missing. Retry the step above and *RESTART* your shell.

You can try if the gem is correctly installed using irb:

```ruby
require 'rubygems'
require 'oci8'
tnsnames = '(DESCRIPTION = (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521)) (CONNECT_DATA = (SID = XE)))'
conn = OCI8.new('SYS', 'sys_pwd', tnsnames)
cursor = conn.exec('SELECT sysdate FROM dual')
while r = cursor.fetch()
puts r.join(',')
end
cursor.close
conn.logoff
```

The code above should not throw any error.

## That's it!

Good job! You deserve any beverage of your choice.

_Note_: This quick install is for local installations useful for developpers. If you want to use it on production systems, it's your call not mine.


References:

* [Ubuntu doc][4]
* [IT-wikipedia][5]

[1]:http://www.oracle.com/technetwork/database/express-edition/overview/index.html
[2]:http://www.ubuntu.com
[3]:http://ruby-oci8.rubyforge.org/en/index.html
[4]:http://doc.ubuntu-fr.org/oracle
[5]:http://www.it-wikipedia.com/web/how-to-install-ruby-oci8-on-ubuntu-server.html
