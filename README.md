# Sample modulefiles for Environment Modules

On RHEL/CentOS 7, the [Environment Modules](http://modules.sourceforge.net/)
utility is provided by the *environment-modules* package. The package also
provides 2 useful tools at `/usr/share/Modules/bin` that facilitate the
conversion of an init shell script into a modulefile:

* createmodule.sh
* createmodule.py

I usually create symbolic links to those tools at `/usr/local/bin`:

```console
# cd /usr/local/bin
# ln -s /usr/share/Modules/bin/createmodule.sh
# ln -s /usr/share/Modules/bin/createmodule.py
```

## Converting Software Collection scriptlets into Environment Modules

We can use the tools to convert a [Software Collection](https://www.softwarecollections.org/en/)'s *enable* scriptlet into a modulefile. For example,
to convert devtoolset-7's *enable* scriptlet into a modulefile:

```console
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

## Converting Intel Parallel Studio XE init scripts into Environment Modules

We can also use the same tools to convert Intel Parallel Studio XE init
scripts into modulefiles. For example:

```console
$ createmodule.sh /opt/intel/vtune_amplifier_2019.1.0.579888/amplxe-vars.sh intel64
Copyright (C) 2009-2018 Intel Corporation. All rights reserved.
Intel(R) VTune(TM) Amplifier 2019 (build 579888)
#%Module 1.0
prepend-path	PATH	/opt/intel/vtune_amplifier_2019.1.0.579888/bin64
prepend-path	PKG_CONFIG_PATH	/opt/intel/vtune_amplifier_2019.1.0.579888/include/pkgconfig/lib64:
setenv		VTUNE_AMPLIFIER_2019_DIR	/opt/intel/vtune_amplifier_2019.1.0.579888
```

Again, I would further edit the produced modulefiles to make them more
readable; and organize the modulefiles in a more logical way.

## References

On RHEL/CentOS 7, the *environment-modules* package provides the *compatibility version* (version 3.2) of the *Environment Modules* utility. The *compatibility version* is implemented in C. A full rewrite of the *Modules* utility in TCL was started in 2012 and has reached maturity. The latest TCL version of *Modules* is 4.2.

* [NERSC - Modules Software Environment](https://www.nersc.gov/users/software/user-environment/modules/)
* [MODULE manual page (C version)](http://modules.sourceforge.net/man/module.html)
* [MODULEFILE manual page (C version)](http://modules.sourceforge.net/man/modulefile.html)
* [Differences between versions 3.2 and 4](https://modules.readthedocs.io/en/stable/diff_v3_v4.html)
* [Environment Modules documentation portal](https://modules.readthedocs.io/en/stable/index.html)
