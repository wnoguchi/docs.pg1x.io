Sphinx
=====================

sudo pip install sphinx

#. `Windowsへのインストール — Python製ドキュメンテーションビルダー、Sphinxの日本ユーザ会 <http://sphinx-users.jp/gettingstarted/install_windows.html#install-easy-install>`_
#. `sphinx-quickstartの詳細説明 — Python製ドキュメンテーションビルダー、Sphinxの日本ユーザ会 <http://sphinx-users.jp/gettingstarted/sphinxquickstart.html>`_
#. `Sphinxの最初の一歩 — Sphinx 1.4.4 ドキュメント <http://docs.sphinx-users.jp/tutorial.html>`_

sphinx-quickstart

::

   Please indicate if you want to use one of the following Sphinx extensions:
   > autodoc: automatically insert docstrings from modules (y/n) [n]: y

::

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

::

   [root@mx1 docs.pg1x.io]# rsync -av /root/docs.pg1x.io/_build/html/ /var/www/docs.pg1x.io/

