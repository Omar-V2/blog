---
layout: post
title: Installing Ruby 1.9.3 on Ubuntu 12.04 Precise Pengolin (without RVM)
tags:
- Mixed
- ruby
- ubuntu
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _wp_old_slug: installing-ruby-1-9-3-on-ubuntu-12-04-precise-pengoli
  dsq_thread_id: '755180513'
---
The new Ubuntu release has just rolled around and with it a slew of new packages. Personally, I'm tracking the development of Ruby quite closely but the default Ruby on Ubuntu ist still the 1.8 series which I can't recommend. Ruby 1.9 has some performance improvements and 1.9.3 in particular a lot of them compared to 1.9.2.

However, as I have elaborated in a <a title="Installing Ruby 1.9.2 on Ubuntu 11.10 Oneiric Ocelot without using RVM" href="http://lenni.info/blog/2011/12/installing-ruby-1-9-2-on-ubuntu-11-10-oneric-ocelot-without-using-rvm/">previous post</a> getting the Ruby 1.9 series on Ubuntu without using RVM instead of 1.8 isn't all that easy. Please read the post if you are interested in the details.

The short version is: You can get Ruby 1.9.3-p0 by installing the <code>ruby-1.9.1</code> package. (The package is called 1.9.1 because that is the ABI version.)

If you want to make Ruby 1.9 the default do the following:
{% highlight bash %}sudo apt-get update

sudo apt-get install ruby1.9.1 ruby1.9.1-dev \
  rubygems1.9.1 irb1.9.1 ri1.9.1 rdoc1.9.1 \
  build-essential libopenssl-ruby1.9.1 libssl-dev zlib1g-dev

sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.1 400 \
         --slave   /usr/share/man/man1/ruby.1.gz ruby.1.gz \
                        /usr/share/man/man1/ruby1.9.1.1.gz \
        --slave   /usr/bin/ri ri /usr/bin/ri1.9.1 \
        --slave   /usr/bin/irb irb /usr/bin/irb1.9.1 \
        --slave   /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.1

# choose your interpreter
# changes symlinks for /usr/bin/ruby , /usr/bin/gem
# /usr/bin/irb, /usr/bin/ri and man (1) ruby
sudo update-alternatives --config ruby
sudo update-alternatives --config gem

# now try
ruby --version{% endhighlight %}

If you want to make this your exclusive Ruby and get rid of Ruby 1.8 follow the <a title="Installing Ruby 1.9.2 on Ubuntu 11.10 Oneiric Ocelot without using RVM" href="http://lenni.info/blog/2011/12/installing-ruby-1-9-2-on-ubuntu-11-10-oneric-ocelot-without-using-rvm/#uninstall">uninstallation instructions</a>.

<strong>Edit</strong>: I found out today that there also is a package called <a href="http://packages.ubuntu.com/precise/ruby1.9.3">ruby1.9.3</a> however that is just a proxy package that doesn't have any files itself and only depends on ruby1.9.1. Aptitude confirms this:
<blockquote>Ruby uses two parallel versioning schemes: the `Ruby library compatibility version' (1.9.1 for this package), which is similar to a library SONAME, and the 'Ruby version' (1.9.3 for this package). Ruby packages in Debian are named using the Ruby library compatibility version, which is sometimes confusing for users who do not follow Ruby development closely. This package depends on the ruby1.9.1 package, and provides compatibility symbolic links from 1.9.3 executables and manual pages to their 1.9.1 counterparts.</blockquote>
There doesn't seem to be a rubygems1.9.3.

