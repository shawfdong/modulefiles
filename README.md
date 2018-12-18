# Sample modulefiles for Environment Modules

On RHEL/CentOS 7, the [Environment Modules](http://modules.sourceforge.net/) 
utility is provided by the *environment-modules* package. The package also 
provides 2 useful tools at `/usr/share/Modules/bin` that facilitate the 
conversion of an init shell script into a modulefile:
* createmodule.sh
* createmodule.py

I usually create symbolic links to those tools at `/usr/local/bin`:
```
# cd /usr/local/bin
# ln -s /usr/share/Modules/bin/createmodule.sh
# ln -s /usr/share/Modules/bin/createmodule.py
```

## Converting Software Collection scriptlets into Environment Modules
We can use the tools to convert a [Software Collection](https://www.softwarecollections.org/en/)'s *enable* scriptlet into a modulefile. For example, 
to convert devtoolset-7's *enable* scriptlet into a modulefile:
```
$ createmodule.sh /opt/rh/devtoolset-7/enable 
#%Module 1.0
prepend-path	INFOPATH	/opt/rh/devtoolset-7/root/usr/share/info
prepend-path	LD_LIBRARY_PATH	/opt/rh/devtoolset-7/root/usr/lib64:/opt/rh/devtoolset-7/root/usr/lib:/opt/rh/devtoolset-7/root/usr/lib64/dyninst:/opt/rh/devtoolset-7/root/usr/lib/dyninst:/opt/rh/devtoolset-7/root/usr/lib64:/opt/rh/devtoolset-7/root/usr/lib
prepend-path	MANPATH	/opt/rh/devtoolset-7/root/usr/share/man
prepend-path	PATH	/opt/rh/devtoolset-7/root/usr/bin
prepend-path	PERL5LIB	/opt/rh/devtoolset-7/root//usr/lib64/perl5/vendor_perl:/opt/rh/devtoolset-7/root/usr/lib/perl5:/opt/rh/devtoolset-7/root//usr/share/perl5/vendor_perl
prepend-path	PYTHONPATH	/opt/rh/devtoolset-7/root/usr/lib64/python2.7/site-packages:/opt/rh/devtoolset-7/root/usr/lib/python2.7/site-packages
setenv		PCP_DIR	/opt/rh/devtoolset-7/root
```
I would then further edit the produced modulefile, by adding help and 
versioning, and turn it into something like [gcc/7.3.1](gcc/7.3.1).
      
