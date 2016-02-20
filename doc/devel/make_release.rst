################
Releasing x86cpu
################

* Review the open list of `x86cpu issues`_.  Check whether there are
  outstanding issues that can be closed, and whether there are any issues that
  should delay the release.  Label them.

* Review and update the release notes.  Review and update the :file:`Changelog`
  file.  Get a partial list of contributors with something like::

      git log 0.2.0.. | grep '^Author' | cut -d' ' -f 2- | sort | uniq

  where ``0.2.0`` was the last release tag name.

  Then manually go over ``git shortlog 0.2.0..`` to make sure the release notes
  are as complete as possible and that every contributor was recognized.

* Use the opportunity to update the ``.mailmap`` file if there are any
  duplicate authors listed from ``git shortlog -ns``.

* Check the copyright years in ``doc/conf.py`` and ``LICENSE``

* Check the output of::

    rst2html.py README.rst > ~/tmp/readme.html

  because this will be the output used by pypi_

* Check `x86cpu travis-ci`_.

* Once everything looks good, you are ready to upload the source release to
  PyPi.  See `setuptools intro`_.  Make sure you have a file
  ``\$HOME/.pypirc``, of form::

    [distutils]
    index-servers =
        pypi

    [pypi]
    username:your.pypi.username
    password:your-password

    [server-login]
    username:your.pypi.username
    password:your-password

* Once everything looks good, tag the release.  This will also set the version
  (we are using versioneer_ to manage versions via git tags)::

    git tag -s 0.3

* Now you can upload the source release to PyPi.  See
  `setuptools intro`_::

    git clean -fxd
    python setup.py register
    python setup.py sdist --formats=gztar,zip upload --sign

  If you're clever, you may have already set up appveyor to build Windows
  wheels, but if not, build Windows wheels, upload to pypi.

* Upload the release commit and tag to github::

    git push
    git push --tags

.. _x86cpu travis-ci: https://travis-ci.org/matthew-brett/x86cpu
.. _x86cpu isses: https://github.com/matthew-brett/x86cpu/issues
.. _versioneer: https://github.com/warner/python-versioneer
.. _setuptools intro:
   http://packages.python.org/an_example_pypi_project/setuptools.html
