=============
Configuration
=============

This plugin provides a clean minimal set of command line options that are added to pytest. For
further control of coverage use a `coverage config file`_.

CLI options:
  --cov [SOURCE]        Path or package name to measure during execution (multi-allowed). Use --cov= to not do any source filtering and record everything.
  --cov-reset           Reset cov sources accumulated in options so far.
  --cov-report TYPE     Type of report to generate: term, term-missing, annotate, html, xml, json, markdown, markdown-append, lcov (multi-allowed). term, term-missing may be followed by ":skip-covered".
                        annotate, html, xml, json, markdown, markdown-append and lcov may be followed by ":DEST" where DEST specifies the output location. Use --cov-report= to not generate any output.
  --cov-config PATH     Config file for coverage. Default: .coveragerc
  --no-cov-on-fail      Do not report coverage if test run fails. Default: False
  --no-cov              Disable coverage report completely (useful for debuggers). Default: False
  --cov-fail-under MIN  Fail if the total coverage is less than MIN.
  --cov-append          Do not delete coverage but append to current. Default: False
  --cov-branch          Enable branch coverage. Can also be specified in the `coverage config file`_ ``[run]`` section.
  --cov-precision COV_PRECISION
                        Override the reporting precision. Can also be specified in the `coverage config file`_ ``[report]`` section.
  --cov-context CONTEXT
                        Dynamic contexts to use. "test" for now.


.. note:: Important Note

    This plugin overrides the ``parallel`` option of coverage. Unless you also run coverage without pytest-cov it's
    pointless to set those options in your ``.coveragerc``.

    If you use the ``--cov=something`` option (with a value) then coverage's ``source`` option will also get overridden.
    If you have multiple sources it might be easier to set those in ``.coveragerc`` and always use ``--cov`` (without a value)
    instead of having a long command line with ``--cov=pkg1 --cov=pkg2 --cov=pkg3 ...``.

    If you use the ``--cov-branch`` option then coverage's ``branch`` option will also get overridden.

If you wish to always run pytest-cov with pytest, you can use ``addopts`` under the ``pytest`` or ``tool:pytest`` section of
your ``setup.cfg``, or the ``tool.pytest.ini_options`` section of your ``pyproject.toml`` file.

For example, in ``setup.cfg``: ::

    [tool:pytest]
    addopts = --cov=<project-name> --cov-report html

Or for ``pyproject.toml``: ::

    [tool.pytest.ini_options]
    addopts = "--cov=<project-name> --cov-report html"


.. note:: Important Note

    The ``--cov`` option has an optional argument. If it's your last option in addopts_ it might eat the next CLI argument, make sure to
    force it to take a blank value if that's what you wanted by using ``--cov=`` (essentially the same as ``--cov=""``).

Caveats
=======

An unfortunate consequence of coverage.py's history is that ``.coveragerc`` is a magic name: it's the default file but it also
means "try to also lookup coverage configuration in ``tox.ini`` or ``setup.cfg``".

In practical terms this means that if you have multiple configuration files around (``tox.ini``, ``pyproject.toml`` or ``setup.cfg``) you
might need to use ``--cov-config`` to make coverage use the correct configuration file.

Also, if you change the working directory and also use subprocesses in a test you might also need to use ``--cov-config`` to make pytest-cov
use the expected configuration file in the subprocess.

.. _`coverage config file`: https://coverage.readthedocs.io/en/latest/config.html
.. _addopts: https://docs.pytest.org/en/stable/reference/reference.html#confval-addopts>
