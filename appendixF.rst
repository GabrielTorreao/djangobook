====================================
Apêndice F: O utilitário django-admin
====================================

``django-admin.py`` é um utilitário de linha de comando do Django para tarefas administrativas.
Este apêndice explica seus muitos poderes.

Você frequentemente acessará ``django-admin.py`` através do pacote ``manage.py``
do projeto. ``manage.py`` é automaticamente criado em cada projeto Django e é um pequeno pacote em volta de
``django-admin.py``. Ele cuida de duas coisas para você
antes de delegar para ``django-admin.py``:

* Ele coloca o pacote do seu projeto no ``sys.path``.

* Ele define a variável de ambiente ``DJANGO_SETTINGS_MODULE`` de modo a
  apontar para o arquivo ``settings.py`` do seu projeto.

O script ``django-admin.py`` deve estar no seu system path se você instalou
Django via seu utilitário ``setup.py``. Se ele não estiver em seu path, você pode encontrá-lo em
``site-packages/django/bin`` dentro da instalação do seu Python. Considere
usar links simbólicos de algum lugar no seu path, como ``/usr/local/bin``.

Usuários de Windows , que não possui a funcionalidade de links simbólicos,
podem copiar ``django-admin.py`` para um local no seu path existente ou editar as definições de
``PATH`` (em Menu Iniciar ~TRA Painel de Controle ~TRA Sistema ~TRA Configurações avançadas do sistema ~TRA
Variáveis de Ambiente) para apontar para o local da instalação.

Geralmente, quando trabalhando com um único projeto Django, é mais fácil usar
``manage.py``. Use ``django-admin.py`` com ``DJANGO_SETTINGS_MODULE`` ou
``--settings`` na linha de comando, se você precisar trocar entre múltiplos
arquivos de definição do Django.

Os exemplos de linha de comando neste apêndice usam ``django-admin.py`` para
ser consistente, mas qualquer exemplopode usar ``manage.py`` da mesma forma.

Uso
=====

Veja como usá-lo::

    django-admin.py <subcommand> [options]
    manage.py <subcommand> [options]

``subcommand`` deve ser um dos subcomandos listados neste apêndice.
``options``, o qual é opicional, deve ser zero ou mais das opções disponiveis
para o subcomando dado.

Obtendo ajuda em tempo de execução
--------------------

Rode ``django-admin.py help`` para exibir uma lista de todos os subcomandos disponíveis.
Rode ``django-admin.py help <subcommand>`` para exibir uma descrição do
subcomando dado e uma lista de suas opções disponíveis.

App names
---------

Muitos subcomandos recebem uma lista de "app names." Um "app name" é o nome base do
pacote contendo seus modelos. Por exemplo, se seu ``INSTALLED_APPS``
contém a string ``'mysite.blog'``, o app name será ``blog``.

Determinando a versão
-----------------------

Rode ``django-admin.py --version`` para exibir a versão atual do Django.

Exemplos de saída::

    1.1
    1.0
    0.96
    0.97-pre-SVN-6069

Exibindo saídas de debug
-----------------------

Use ``--verbosity`` para especificar a quantidade de notificações e informação de debug
que ``django-admin.py`` deve imprimir no console.

Subcomandos disponíveis
=====================

cleanup
-------

Pode ser executado como um cronjob ou diretamente para limpar dados antigos do banco de dados
(apenas sessões expiradas no momento).

compilemessages
---------------

Compila .po arquivos criados com ``makemessages`` para .mo arquivos para uso com
o suorte gettext embutido. Veja o capítulo 19.

--locale
~~~~~~~~

Use a opção ``--locale`` ou ``-l`` para especificar o local do processo.
Caso não seja fornecido, todos os locais são processados.

Exemplo de uso::

    django-admin.py compilemessages --locale=br_PT

createcachetable
----------------

Cria uma tabela de cache com o nome fornecido para uso com a cache do banco de dados
backend. Veja o Capítulo 15.

Exemplo de uso::

    django-admin.py createcachetable my_cache_table

createsuperuser
---------------

Cria uma conta de supeusuário (um usuário que tem todas as permissões). Isto é
útil se você precisa criar uma conta de superusuário inicial porém não
o fêz durante ``syncdb``, ou se você precisa gerar programaticamnete contas de superusuário
para o seu site(s).

Quando executado de forma interativa, este comando irá solicitar uma senha para
a nova conta de superusuário. Quando executado de forma não-interativa, nenhuma senha
será definida, e a conta de superusuário não será capaz de conectar até que
uma senha seja manualmente definida para ela.

O nome de usuário e o endereço de e-mail da nova conta podem ser fornecidos
usando os argumentos ``--username`` e ``--email`` na linha de
comando. Se algum dos dois não for fornecido, ``createsuperuser`` irá solicitá-los
quando estiver executando de forma interativa.

Este comando só vais estar disponível se o sistema de autenticação do Django
(``django.contrib.auth``) estiver em ``INSTALLED_APPS``. Veja o Capítulo 14.

dbshell
-------

Executa o cliente da linha de comando para o engine de banco de dados especificado na sua
configuraçõa ``DATABASE_ENGINE``, com os parâmetros de conexão especificados nas suas configurações
``DATABASE_USER``, ``DATABASE_PASSWORD``, etc.

* Para PostgreSQL, isso executa o cliente da linha de comando ``psql``.
* Para MySQL, isso executa o cliente da linha de comando ``mysql``.
* Para SQLite, isso executa o cliente da linha de comando ``sqlite3``.

Este comando assume que os programas estão em seu ``PATH`` Tal que uma simples chamada para
o nome do programa (``psql``, ``mysql``, ``sqlite3``) irá encontrar o programa no
local correto. Não há nenhuma maneira de especificar o local do programa
manualmente.

diffsettings
------------

Exibe as diferenças entre o atual arquivo de configurações e o arquivo de configurações
padrão do Django.

Configurações que não aparecem no padrão são seguidas por ``"###"``. Por
exemplo, a configuração padrão não define ``ROOT_URLCONF``, então
``ROOT_URLCONF`` é seguido por ``"###"`` na saída de ``diffsettings``.

Note que as configurações padrão do Django estão em ``django/conf/global_settings.py``,
Se você tiver a curiosidade de ver a lista completa de padrões.

dumpdata
--------

Encaminha para a saída padrão todos os dados no banco de dados associados à aplicação
nomeada.

Se nenhum nome de aplicação for fornecido, todos os aplicativos instalados serão despejados.

A saída de ``dumpdata`` pode ser usada como entrada para ``loaddata``.

Note que ``dumpdata`` usa o gerenciador padrão no modelo para selecionar os
registros a serem exportados. Se você estiver usando um gerenciador personalizado como gerenciador padrão
o qual filtra alguns dos registros disponíveis, nem todos os objetos serão despejados.

Exemplo de uso::

    django-admin.py dumpdata books

Use a opção ``--exclude`` para excuir um aplicativo específico das aplicações
cujos conteúdos são de saída. Por exemplo, para excluir especificamente
o aplicativo `auth` da saída, você chamaria::

    django-admin.py dumpdata --exclude=auth

Se você quiser excluir várias aplicações, use multiplas diretrizes ``--exclude``::

    django-admin.py dumpdata --exclude=auth --exclude=contenttypes

Por padrão, ``dumpdata`` irá formatar sua saída em JSON, porém você pode usar
a opção ``--format`` para especificar outro formato. Formatos suportados atualmente
estão listados em :ref:`serialization-formats`.

Por padrão, ``dumpdata`` irá imprimir todos os dados de saída em uma única linha. Isto não é
fácil para pessoas ler, então você pode usar a opção ``--indent`` para imprimir a saída
de maneira organizada com um número de espaços de identação.

Além de especificar os nomes dos aplicativos, você pode fornecer uma lista
de modelos individuais, na forma de ``appname.Model``. Se você especificar o nome
de um modelo para ``dumpdata``, a saída extraida será restringida a este modelo,
em vez de toda a aplicação. Você também pode misturar nomes de aplicativos
e nomes de modelos.

flush
-----

Returns the database to the state it was in immediately after syncdb was
executed. This means that all data will be removed from the database, any
post-synchronization handlers will be re-executed, and the ``initial_data``
fixture will be re-installed.

Use the ``--noinput`` option to suppress all user prompting, such as "Are
you sure?" confirmation messages. This is useful if ``django-admin.py`` is
being executed as an unattended, automated script.

inspectdb
---------

Introspects the database tables in the database pointed-to by the
``DATABASE_NAME`` setting and outputs a Django model module (a ``models.py``
file) to standard output.

Use this if you have a legacy database with which you'd like to use Django.
The script will inspect the database and create a model for each table within
it.

As you might expect, the created models will have an attribute for every field
in the table. Note that ``inspectdb`` has a few special cases in its field-name
output:

* If ``inspectdb`` cannot map a column's type to a model field type, it'll
  use ``TextField`` and will insert the Python comment
  ``'This field type is a guess.'`` next to the field in the generated
  model.

* If the database column name is a Python reserved word (such as
  ``'pass'``, ``'class'`` or ``'for'``), ``inspectdb`` will append
  ``'_field'`` to the attribute name. For example, if a table has a column
  ``'for'``, the generated model will have a field ``'for_field'``, with
  the ``db_column`` attribute set to ``'for'``. ``inspectdb`` will insert
  the Python comment
  ``'Field renamed because it was a Python reserved word.'`` next to the
  field.

This feature is meant as a shortcut, not as definitive model generation. After
you run it, you'll want to look over the generated models yourself to make
customizations. In particular, you'll need to rearrange models' order, so that
models that refer to other models are ordered properly.

Primary keys are automatically introspected for PostgreSQL, MySQL and
SQLite, in which case Django puts in the ``primary_key=True`` where
needed.

``inspectdb`` works with PostgreSQL, MySQL and SQLite. Foreign-key detection
only works in PostgreSQL and with certain types of MySQL tables.

loaddata <fixture fixture ...>
------------------------------

Searches for and loads the contents of the named fixture into the database.

What's a "fixture"?
~~~~~~~~~~~~~~~~~~~

A *fixture* is a collection of files that contain the serialized contents of
the database. Each fixture has a unique name, and the files that comprise the
fixture can be distributed over multiple directories, in multiple applications.

Django will search in three locations for fixtures:

1. In the ``fixtures`` directory of every installed application
2. In any directory named in the ``FIXTURE_DIRS`` setting
3. In the literal path named by the fixture

Django will load any and all fixtures it finds in these locations that match
the provided fixture names.

If the named fixture has a file extension, only fixtures of that type
will be loaded. For example::

    django-admin.py loaddata mydata.json

would only load JSON fixtures called ``mydata``. The fixture extension
must correspond to the registered name of a
serializer (e.g., ``json`` or ``xml``). For more on serializers, see the Django
docs.

If you omit the extensions, Django will search all available fixture types
for a matching fixture. For example::

    django-admin.py loaddata mydata

would look for any fixture of any fixture type called ``mydata``. If a fixture
directory contained ``mydata.json``, that fixture would be loaded
as a JSON fixture.

The fixtures that are named can include directory components. These
directories will be included in the search path. For example::

    django-admin.py loaddata foo/bar/mydata.json

would search ``<appname>/fixtures/foo/bar/mydata.json`` for each installed
application,  ``<dirname>/foo/bar/mydata.json`` for each directory in
``FIXTURE_DIRS``, and the literal path ``foo/bar/mydata.json``.

When fixture files are processed, the data is saved to the database as is.
Model defined ``save`` methods and ``pre_save`` signals are not called.

Note that the order in which fixture files are processed is undefined. However,
all fixture data is installed as a single transaction, so data in
one fixture can reference data in another fixture. If the database backend
supports row-level constraints, these constraints will be checked at the
end of the transaction.

The ``dumpdata`` command can be used to generate input for ``loaddata``.

Compressed fixtures
~~~~~~~~~~~~~~~~~~~

Fixtures may be compressed in ``zip``, ``gz``, or ``bz2`` format. For example::

    django-admin.py loaddata mydata.json

would look for any of ``mydata.json``, ``mydata.json.zip``,
``mydata.json.gz``, or ``mydata.json.bz2``.  The first file contained within a
zip-compressed archive is used.

Note that if two fixtures with the same name but different
fixture type are discovered (for example, if ``mydata.json`` and
``mydata.xml.gz`` were found in the same fixture directory), fixture
installation will be aborted, and any data installed in the call to
``loaddata`` will be removed from the database.

.. admonition:: MySQL and Fixtures

    Unfortunately, MySQL isn't capable of completely supporting all the
    features of Django fixtures. If you use MyISAM tables, MySQL doesn't
    support transactions or constraints, so you won't get a rollback if
    multiple fixture files are found, or validation of fixture data fails.

    If you use InnoDB tables, you won't be able to have any forward
    references in your data files -- MySQL doesn't provide a mechanism to
    defer checking of row constraints until a transaction is committed.

makemessages
------------

Runs over the entire source tree of the current directory and pulls out all
strings marked for translation. It creates (or updates) a message file in the
conf/locale (in the django tree) or locale (for project and application)
directory. After making changes to the messages files you need to compile them
with ``compilemessages`` for use with the builtin gettext support. See Chapter
19 for details.

--all
~~~~~

Use the ``--all`` or ``-a`` option to update the message files for all
available languages.

Example usage::

    django-admin.py makemessages --all

--extension
~~~~~~~~~~~

Use the ``--extension`` or ``-e`` option to specify a list of file extensions
to examine (default: ".html").

Example usage::

    django-admin.py makemessages --locale=de --extension xhtml

Separate multiple extensions with commas or use -e or --extension multiple times::

    django-admin.py makemessages --locale=de --extension=html,txt --extension xml

--locale
~~~~~~~~

Use the ``--locale`` or ``-l`` option to specify the locale to process.

Example usage::

    django-admin.py makemessages --locale=br_PT

--domain
~~~~~~~~

Use the ``--domain`` or ``-d`` option to change the domain of the messages files.
Currently supported:

* ``django`` for all ``*.py`` and ``*.html`` files (default)
* ``djangojs`` for ``*.js`` files

.. _django-admin-reset:

reset <appname appname ...>
---------------------------

Executes the equivalent of ``sqlreset`` for the given app name(s).

--noinput
~~~~~~~~~

Use the ``--noinput`` option to suppress all user prompting, such as
"Are you sure?" confirmation messages. This is useful if ``django-admin.py``
is being executed as an unattended, automated script.

runfcgi [options]
-----------------

Starts a set of FastCGI processes suitable for use with any Web server that
supports the FastCGI protocol. See Chapter 12 for details. Requires the Python
FastCGI module from flup: http://www.saddi.com/software/flup/

runserver
---------

Starts a lightweight development Web server on the local machine. By default,
the server runs on port 8000 on the IP address 127.0.0.1. You can pass in an
IP address and port number explicitly.

If you run this script as a user with normal privileges (recommended), you
might not have access to start a port on a low port number. Low port numbers
are reserved for the superuser (root).

DO NOT USE THIS SERVER IN A PRODUCTION SETTING. It has not gone through
security audits or performance tests. (And that's how it's gonna stay. We're in
the business of making Web frameworks, not Web servers, so improving this
server to be able to handle a production environment is outside the scope of
Django.)

The development server automatically reloads Python code for each request, as
needed. You don't need to restart the server for code changes to take effect.

When you start the server, and each time you change Python code while the
server is running, the server will validate all of your installed models. (See
the ``validate`` command below.) If the validator finds errors, it will print
them to standard output, but it won't stop the server.

You can run as many servers as you want, as long as they're on separate ports.
Just execute ``django-admin.py runserver`` more than once.

Note that the default IP address, 127.0.0.1, is not accessible from other
machines on your network. To make your development server viewable to other
machines on the network, use its own IP address (e.g. ``192.168.2.1``) or
``0.0.0.0`` (which you can use if you don't know what your IP address is
on the network).

Use the ``--adminmedia`` option to tell Django where to find the various CSS
and JavaScript files for the Django admin interface. Normally, the development
server serves these files out of the Django source tree magically, but you'd
want to use this if you made any changes to those files for your own site.

Example usage::

    django-admin.py runserver --adminmedia=/tmp/new-admin-style/

Use the ``--noreload`` option to disable the use of the auto-reloader. This
means any Python code changes you make while the server is running will *not*
take effect if the particular Python modules have already been loaded into
memory.

Example usage::

    django-admin.py runserver --noreload

Examples of using different ports and addresses
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Port 8000 on IP address 127.0.0.1::

	django-admin.py runserver

Port 8000 on IP address 1.2.3.4::

	django-admin.py runserver 1.2.3.4:8000

Port 7000 on IP address 127.0.0.1::

    django-admin.py runserver 7000

Port 7000 on IP address 1.2.3.4::

    django-admin.py runserver 1.2.3.4:7000

Serving static files with the development server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, the development server doesn't serve any static files for your site
(such as CSS files, images, things under ``MEDIA_URL`` and so forth).

shell
-----

Starts the Python interactive interpreter.

Django will use IPython (http://ipython.scipy.org/), if it's installed. If you
have IPython installed and want to force use of the "plain" Python interpreter,
use the ``--plain`` option, like so::

    django-admin.py shell --plain

sql <appname appname ...>
-------------------------

Prints the CREATE TABLE SQL statements for the given app name(s).

sqlall <appname appname ...>
----------------------------

Prints the CREATE TABLE and initial-data SQL statements for the given app name(s).

Refer to the description of ``sqlcustom`` for an explanation of how to
specify initial data.

sqlclear <appname appname ...>
------------------------------

Prints the DROP TABLE SQL statements for the given app name(s).

sqlcustom <appname appname ...>
-------------------------------

Prints the custom SQL statements for the given app name(s).

For each model in each specified app, this command looks for the file
``<appname>/sql/<modelname>.sql``, where ``<appname>`` is the given app name and
``<modelname>`` is the model's name in lowercase. For example, if you have an
app ``news`` that includes a ``Story`` model, ``sqlcustom`` will attempt
to read a file ``news/sql/story.sql`` and append it to the output of this
command.

Each of the SQL files, if given, is expected to contain valid SQL. The SQL
files are piped directly into the database after all of the models'
table-creation statements have been executed. Use this SQL hook to make any
table modifications, or insert any SQL functions into the database.

Note that the order in which the SQL files are processed is undefined.

sqlflush
--------

Prints the SQL statements that would be executed for the `flush`_ command.

sqlindexes <appname appname ...>
--------------------------------

Prints the CREATE INDEX SQL statements for the given app name(s).

sqlreset <appname appname ...>
------------------------------

Prints the DROP TABLE SQL, then the CREATE TABLE SQL, for the given app name(s).

sqlsequencereset <appname appname ...>
--------------------------------------

Prints the SQL statements for resetting sequences for the given app name(s).

startapp <appname>
------------------

Creates a Django app directory structure for the given app name in the current
directory.

startproject <projectname>
--------------------------

Creates a Django project directory structure for the given project name in the
current directory.

This command is disabled when the ``--settings`` option to
``django-admin.py`` is used, or when the environment variable
``DJANGO_SETTINGS_MODULE`` has been set. To re-enable it in these
situations, either omit the ``--settings`` option or unset
``DJANGO_SETTINGS_MODULE``.

syncdb
------

Creates the database tables for all apps in ``INSTALLED_APPS`` whose tables
have not already been created.

Use this command when you've added new applications to your project and want to
install them in the database. This includes any apps shipped with Django that
might be in ``INSTALLED_APPS`` by default. When you start a new project, run
this command to install the default apps.

.. admonition:: Syncdb will not alter existing tables

   ``syncdb`` will only create tables for models which have not yet been
   installed. It will *never* issue ``ALTER TABLE`` statements to match
   changes made to a model class after installation. Changes to model classes
   and database schemas often involve some form of ambiguity and, in those
   cases, Django would have to guess at the correct changes to make. There is
   a risk that critical data would be lost in the process.

   If you have made changes to a model and wish to alter the database tables
   to match, use the ``sql`` command to display the new SQL structure and
   compare that to your existing table schema to work out the changes.

If you're installing the ``django.contrib.auth`` application, ``syncdb`` will
give you the option of creating a superuser immediately.

``syncdb`` will also search for and install any fixture named ``initial_data``
with an appropriate extension (e.g. ``json`` or ``xml``). See the
documentation for ``loaddata`` for details on the specification of fixture
data files.

--noinput
~~~~~~~~~

Use the ``--noinput`` option to suppress all user prompting, such as
"Are you sure?" confirmation messages. This is useful if ``django-admin.py``
is being executed as an unattended, automated script.

test
----

Runs tests for all installed models. See the Django documentation for more
on testing.

--noinput
~~~~~~~~~

Use the ``--noinput`` option to suppress all user prompting, such as
"Are you sure?" confirmation messages. This is useful if ``django-admin.py``
is being executed as an unattended, automated script.

testserver <fixture fixture ...>
--------------------------------

Runs a Django development server (as in ``runserver``) using data from the
given fixture(s).

For more, see the Django documentation.

validate
--------

Validates all installed models (according to the ``INSTALLED_APPS`` setting)
and prints validation errors to standard output.

Default options
===============

Although some subcommands may allow their own custom options, every subcommand
allows for the following options:

--pythonpath
------------

Example usage::

    django-admin.py syncdb --pythonpath='/home/djangoprojects/myproject'

Adds the given filesystem path to the Python import search path. If this
isn't provided, ``django-admin.py`` will use the ``PYTHONPATH`` environment
variable.

Note that this option is unnecessary in ``manage.py``, because it takes care of
setting the Python path for you.

--settings
----------

Example usage::

    django-admin.py syncdb --settings=mysite.settings

Explicitly specifies the settings module to use. The settings module should be
in Python package syntax, e.g. ``mysite.settings``. If this isn't provided,
``django-admin.py`` will use the ``DJANGO_SETTINGS_MODULE`` environment
variable.

Note that this option is unnecessary in ``manage.py``, because it uses
``settings.py`` from the current project by default.

--traceback
-----------

Example usage::

    django-admin.py syncdb --traceback

By default, ``django-admin.py`` will show a simple error message whenever an
error occurs. If you specify ``--traceback``, ``django-admin.py``  will
output a full stack trace whenever an exception is raised.

--verbosity
-----------

Example usage::

    django-admin.py syncdb --verbosity 2

Use ``--verbosity`` to specify the amount of notification and debug information
that ``django-admin.py`` should print to the console.

* ``0`` means no output.
* ``1`` means normal output (default).
* ``2`` means verbose output.

Extra niceties
==============

Syntax coloring
---------------

The ``django-admin.py`` / ``manage.py`` commands that output SQL to standard
output will use pretty color-coded output if your terminal supports
ANSI-colored output. It won't use the color codes if you're piping the
command's output to another program.

Bash completion
---------------

If you use the Bash shell, consider installing the Django bash completion
script, which lives in ``extras/django_bash_completion`` in the Django
distribution. It enables tab-completion of ``django-admin.py`` and
``manage.py`` commands, so you can, for instance...

* Type ``django-admin.py``.
* Press [TAB] to see all available options.
* Type ``sql``, then [TAB], to see all available options whose names start
  with ``sql``.
