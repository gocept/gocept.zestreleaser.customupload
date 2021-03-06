=================
Custom egg upload
=================

This package provides a plugin for ``zest.releaser`` that offers to upload the
released egg via SCP, SFTP or HTTP(S) PUT (WebDAV) to a custom location (instead of or
in addition to PyPI).

To use, add a section to your ``~/.pypirc`` like the following::

    [gocept.zestreleaser.customupload]
    gocept = scp://download.gocept.com//var/www/packages
    gocept.special = http://dav.gocept.com/special
    gocept.sftp = sftp://repo@repo.gocept.com/home/repo/eggs

If the name of the package being released starts with one of the keys in that
section (longest match wins, case insensitive), you will be prompted whether to
upload the egg (that was created by zest.releaser by checking out the tag) to
the given server.

If the server is using non-standard ports for scp or sftp (the standard port is
22 in both cases), you can signal it including the port in the server url, as follows::

    [gocept.zestreleaser.customupload]
    gocept.scp = scp://download.gocept.com:6522//var/www/packages
    gocept.sftp = sftp://repo@repo.gocept.com:7522/home/repo/eggs

In the first case, the scp will be done to the port 6522 instead to the standard 22
and in the second case the sftp will connect to port 7522 instead to 22

Options for HTTP(S) PUT (WebDAV)
================================

HTTP(S) PUT (WebDAV) is implemented using `curl` to add options to `curl`,
e. g. to disable certificate checks, add the options in front of the URL
like this::

    [gocept.zestreleaser.customupload]
    gocept.special = --insecure https://dav.gocept.com/special


Uploading Documentation
=======================

In addition to uploading the egg, the plugin also offers to upload generated
documentation.

To use this functionality, create a ``~/.zestreleaserrc`` that contains
something like the following::

    [gocept.zestreleaser.customupload.doc]
    gocept = docs.gocept.com:/var/www/doc

If the name of the package being released starts with one of the keys in that
section, the plugin will run ``./bin/doc`` to generate the documentation and
then prompt to upload the contents of ``./build/doc``. The upload destination
is assembled from the configured prefix, the package name and version. In the
example, the upload location would be ``/var/www/doc/gocept.foo/1.3``.


Development
===========

The source code is available in the Mercurial repository at
https://github.com/gocept/gocept.zestreleaser.customupload

Please report any bugs you find at
https://github.com/gocept/gocept.zestreleaser.customupload/issues
