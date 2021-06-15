# GLPI installation documentation

[![Documentation Status](https://readthedocs.org/projects/glpi-install/badge/?version=latest)](http://glpi-install.readthedocs.io/en/latest/?badge=latest)

Current documentation is built on top of [Sphinx documentation generator](http://sphinx-doc.org/). It is released under the terms of the <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons BY-NC-ND 4.0 International License</a>.

## View it online!

[GLPI installation documentation is currently visible on ReadTheDocs](http://glpi-install.rtfd.io/).

## Run it!

You'll have to install [Python Sphinx](http://sphinx-doc.org/) 1.3 minimum.

If your distribution does not provide this version, you could use a `virtualenv`:
```
$ virtualenv /path/to/virtualenv/files
$ /path/to/virtualenv/bin/activate
$ pip install sphinx
```

Once all has been successfully installed, just run the following to build the documentation:
```
$ make html
```

Results will be avaiable in the `build/html` directory :)

Note that it actually uses the default theme, which differs locally and on readthedocs system.

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a>
