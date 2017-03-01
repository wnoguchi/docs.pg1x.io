Sphinx
=====================

::

   sudo pip install sphinx

#. `Windowsへのインストール — Python製ドキュメンテーションビルダー、Sphinxの日本ユーザ会 <http://sphinx-users.jp/gettingstarted/install_windows.html#install-easy-install>`_
#. `sphinx-quickstartの詳細説明 — Python製ドキュメンテーションビルダー、Sphinxの日本ユーザ会 <http://sphinx-users.jp/gettingstarted/sphinxquickstart.html>`_
#. `Sphinxの最初の一歩 — Sphinx 1.4.4 ドキュメント <http://docs.sphinx-users.jp/tutorial.html>`_

sphinx-quickstart

::

   Please indicate if you want to use one of the following Sphinx extensions:
   > autodoc: automatically insert docstrings from modules (y/n) [n]: y

.. code-block:: shell-session

   [root@mx1 docs.pg1x.io]# make html
   Running Sphinx v1.5.1
   making output directory...
   loading translations [ja]... done
   loading pickled environment... not yet created
   building [mo]: targets for 0 po files that are out of date
   building [html]: targets for 1 source files that are out of date
   updating environment: 1 added, 0 changed, 0 removed
   reading sources... [100%] index

   looking for now-outdated files... none found
   pickling environment... done
   checking consistency... done
   preparing documents... done
   writing output... [100%] index

   generating indices... genindex
   writing additional pages... search
   copying static files... done
   copying extra files... done
   dumping search index in Japanese (code: ja) ... done
   dumping object inventory... done
   build succeeded.

   Build finished. The HTML pages are in _build/html.

.. code-block:: shell-session

   [root@mx1 docs.pg1x.io]# rsync -av /root/docs.pg1x.io/_build/html/ /var/www/docs.pg1x.io/

===============================
Upgrade Sphinx
===============================

.. code-block:: shell-session

   ➜  src git:(master) pip install --upgrade sphinx
   Collecting sphinx
     Downloading Sphinx-1.5.2-py2.py3-none-any.whl (1.6MB)
       100% |████████████████████████████████| 1.6MB 280kB/s
   Requirement already up-to-date: babel!=2.0,>=1.3 in /usr/local/lib/python2.7/site-packages (from sphinx)
   Collecting requests>=2.4.0 (from sphinx)
     Downloading requests-2.12.5-py2.py3-none-any.whl (576kB)
       100% |████████████████████████████████| 583kB 111kB/s
   Collecting Jinja2>=2.3 (from sphinx)
     Downloading Jinja2-2.9.4-py2.py3-none-any.whl (274kB)
       100% |████████████████████████████████| 276kB 210kB/s
   Requirement already up-to-date: imagesize in /usr/local/lib/python2.7/site-packages (from sphinx)
   Requirement already up-to-date: six>=1.5 in /usr/local/lib/python2.7/site-packages (from sphinx)
   Requirement already up-to-date: Pygments>=2.0 in /usr/local/lib/python2.7/site-packages (from sphinx)
   Requirement already up-to-date: snowballstemmer>=1.1 in /usr/local/lib/python2.7/site-packages (from sphinx)
   Requirement already up-to-date: alabaster<0.8,>=0.7 in /usr/local/lib/python2.7/site-packages (from sphinx)
   Collecting docutils>=0.11 (from sphinx)
     Downloading docutils-0.13.1-py2-none-any.whl (537kB)
       100% |████████████████████████████████| 542kB 96kB/s
   Collecting pytz>=0a (from babel!=2.0,>=1.3->sphinx)
     Downloading pytz-2016.10-py2.py3-none-any.whl (483kB)
       100% |████████████████████████████████| 491kB 74kB/s
   Requirement already up-to-date: MarkupSafe>=0.23 in /usr/local/lib/python2.7/site-packages (from Jinja2>=2.3->sphinx)
   Installing collected packages: requests, Jinja2, docutils, sphinx, pytz
     Found existing installation: Jinja2 2.8
       Uninstalling Jinja2-2.8:
         Successfully uninstalled Jinja2-2.8
     Found existing installation: docutils 0.12
       Uninstalling docutils-0.12:
         Successfully uninstalled docutils-0.12
     Found existing installation: Sphinx 1.4.9
       Uninstalling Sphinx-1.4.9:
         Successfully uninstalled Sphinx-1.4.9
     Found existing installation: pytz 2016.7
       Uninstalling pytz-2016.7:
         Successfully uninstalled pytz-2016.7
   Successfully installed Jinja2-2.9.4 docutils-0.13.1 pytz-2016.10 requests-2.12.5 sphinx-1.5.2
   You are using pip version 8.1.2, however version 9.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.

======================================
Syntax Highlight
======================================

#. `Supported languages — Pygments <http://pygments.org/languages/>`_

Network config
----------------------------

#. `nemith/pygments-routerlexers: Lexers for router configurations for Pygments <https://github.com/nemith/pygments-routerlexers>`_

