# Pyflame

Pyflame is a tool for generating
[flame graphs](https://github.com/brendangregg/FlameGraph) for Python processes.
Pyflame works by using the
[ptrace(2)](http://man7.org/linux/man-pages/man2/ptrace.2.html) system call to
analyze the currently-executing stack trace for a Python process.

## Installing

To build Pyflame you will need a C++ compiler with C++11 support, and GNU
Autotools ([GNU Autoconf](https://www.gnu.org/software/autoconf/autoconf.html)
and [GNU Automake](https://www.gnu.org/software/automake/automake.html)). Then
you can build it with an invocation like:

```bash
./autogen.sh
./configure      # plus any options like --prefix
make
make install
```

If you'd like to build a Debian package there's already a `debian/` directory at
the root of this project. We'd like to remove this, as per the
[upstream Debian packaging guidelines](https://wiki.debian.org/UpstreamGuide).
If you can help get this project packaged in Debian please let us know.

## Usage

After compiling Pyflame you'll get a small executable called `pyflame`. The most
basic usage is:

```bash
pyflame PID
```

You can also change the sample time and sampling frequency:

```bash
# profile for 60 seconds, sampling every 10ms
pyflame -s 60 -r 0.10 PID
```

You'll need appropriate permissions to `PTRACE_ATTACH` the process. Typically
this means that you'll need to invoke `pyflame` as root, or as the same user as
the process you're trying to profile. If you have errors running it as the
correct user then you probably have `ptrace_scope` set to a value that's too
restrictive. This is the default in Debian Jessie.

To see the current value:

```bash
sysctl kernel.yama.ptrace_scope
```

If you see a value other than 0 you likely need to change it:

```bash
sudo sysctl kernel.yama.ptrace_scope=0
```

If you'd like to know more about this feature please read
[the relevant kernel documentation](https://www.kernel.org/doc/Documentation/security/Yama.txt).

## License

Pyflame is free software distributed under the terms of the
[Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0). You
should receive a copy of this license along with Pyflame.
