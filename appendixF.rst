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

Retorna o banco de dados para o estado que estava, imediatamente depois que syncdb
foi executado. Isto significa que todos os dados serão removidos do banco de daods, quaisquer
manipuladores de pós-sincronização serão re-executados, e a fixação ``initial_data``
será reinstalada.

Use a opção ``--noinput`` para suprimir todo prompt ao usuário, tais como mensagens de
confirmação "Are you sure?". Isto é útil se ``django-admin.py`` está sendo executado
como um script aoutônomo.

inspectdb
---------

Introspecta as tabelas do banco de dados no banco de dados apontado pela
configuração ``DATABASE_NAME`` e retorna um módulo no modelo Django (um arquivo ``models.py``)
para a saída padrão.

Use isto se você tem um banco de dados legacy com o qual você gostaria de usar o Django.
O script irá inspecionar o banco de dados e criar um modelo para cada tabela dentro dele.

Assim como esperado, os modelos criados terão um atributo para cada campo da
tabela. Note que ``inspectdb`` tem alguns casos especiais em seu campo de nome
na saída:

* Caso ``inspectdb`` não consiga mapear o tipo de uma coluna para um campo de tipo
  no modelo, ele usará ``TextField`` e irá inserir o comentário Python
  ``'This field type is a guess.'`` ao lado do campo no modelo gerado.

* Se o nome da coluna é um nome reservado de Python (Tais como
  ``'pass'``, ``'class'`` ou ``'for'``), ``inspectdb`` anexará
  ``'_field'`` ao nome do atributo. Por exemplo, se a tabela tem uma coluna
  ``'for'``, o modelo gerado terá um campo ``'for_field'``, com o atributo
  ``db_column`` definido como ``'for'``. ``inspectdb`` irá inserir o
  comentário Python
  ``'Field renamed because it was a Python reserved word.'`` ao lado do
  campo.

Este recurso foi definido como um atalho, não como um meio definitivo de gerar modelos.
depois de executá-lo, você deve revisar os modelos gerados para fazer alterações.
Em particular, você deve reorganizar a ordem dos modelos, para que modelos os quais
se referem a outros modelos estejam propriamente ordenados.

Chaves primárias são automaticamente introspectadas para PostgreSQL, MySQL e
SQLite, nesse caso, Django coloca ``primary_key=True`` onde for necessário.

``inspectdb`` funciona com PostgreSQL, MySQL and SQLite. Detecção de chaves
estrangeiras só funciona em PostgreSQL e com certos tipos de tabelas MySQL.

loaddata <fixture fixture ...>
------------------------------

Procura e carrega o conteúdo da fixture nomeada no banco de dados.

O que é uma "fixture"?
~~~~~~~~~~~~~~~~~~~

Uma *fixture* é um conjunto de arquivos que contêm o conteúdo do banco de dados
serializado. Cada fixture tem um nome único, e os arquivos que compõem a fixture
podem ser distribuidos em vários diretórios, em várias aplicações.

Django irá procurar em três luagres por fixtures:

1. No diretório ``fixtures`` de cada aplicação instalada
2. Em qualquer diretório nomeado na configuração ``FIXTURE_DIRS``
3. No caminho literal nomeado pela fixture

Django carregará todas e qualquer fixtures que encontrar nestes três locais
as quais correspondem aos nomes fornecidos.

Caso a fixture nomeada possui uma extensão de arquivo, apenas fixtures daquele tipo
serão carregadas. Por exemplo::

    django-admin.py loaddata mydata.json

apenas carregará fixtures JSON chamadas ``mydata``. A extensão da fixture
deve corresponder ao nome registrado de um serializador
(e.g., ``json`` or ``xml``). Para mais sobre serializadores, consulte o Django
docs.

Se você omitir as extensões, Django irá procurar todos os tipos de fixture disponíveis
por uma fixture correspondente. Por exemplo::

    django-admin.py loaddata mydata

irá procurar por qualquer fixture de qualquer tipo chamada ``mydata``. Caso
um diretório de fixtures contenha ``mydata.json``, essa fixture será carregada
como uma fixture JSON.

As fixtures nomeadas podem incluir componentes de diretório. Estes
diretórios serão incluidos no caminho da busca. Por exemplo::

    django-admin.py loaddata foo/bar/mydata.json

irá procurar ``<appname>/fixtures/foo/bar/mydata.json`` para cada aplicação
instalada,  ``<dirname>/foo/bar/mydata.json`` para cada diretório em
``FIXTURE_DIRS``, e o caminho literal ``foo/bar/mydata.json``.

Quando arquivos fixture são prcessados, os dados são salvos no banco de dados como estão.
Métodos definidos pelo modelo ``save`` e sinais ``pre_save`` não são chamados.

Note que a ordem em que cada arquivo fixture é processado não é definida. Porém,
todos os dados da fixture são instalados como uma transação única, para que os
dados de uma fixture possam referenciar dados de outra fixture. Se o banco de dados backend
suporta restrições de nivel de linha, estas restrições serão checadas ao fim da
transação.

O comando ``dumpdata`` pode ser usado para gerar entradas para ``loaddata``.

Fixtures compactadas
~~~~~~~~~~~~~~~~~~~

Fixtures podem ser compactadas em formato ``zip``, ``gz``, or ``bz2``. Por exemplo::

    django-admin.py loaddata mydata.json

irá procurar por qualquer ``mydata.json``, ``mydata.json.zip``,
``mydata.json.gz``, ou ``mydata.json.bz2``. O primeiro arquivo contido em
um arquivo compactado em formato zip é usado.

Note que se duas fixtures com o mesmo nome mas com tipos de fixtures
diferentes forem encontradas (por exemplo, se ``mydata.json`` e
``mydata.xml.gz`` forem encontradas no mesmo diretório de fixtures),
a instalação de fixtures será abortada, e qualquer dados instalados na chamada de
``loaddata`` serão removidos do banco de dados.

.. advertência:: MySQL e Fixtures

    Infelizmente, MySQL não é capaz de prover suporte completo a todos os
    recursos das fixtures de Django. Se você usa tabelas MyISAM, MySQL não
    suporta transações ou restrições, então você não vai ter um rollback caso
    vários arquivos de fixtures sejam encontrados, ou a validação dos dados das fixtures falhar.

    Se você usa tabelas InnoDB, você não será capaz de ter quaisquer referências
    à frente em seus arquivos de dados -- MySQL não fornece um mecanismo para
    deferir a verificação de restrições de linha até que a transação seja concluida.

makemessages
------------

Percorre toda a árvore fonte do diretório atual e tira todas as
strings marcadas para tradução. Isto cria (ou atualiza) o arquivo de mensagens em
conf/locale (na árvore django) ou no diretório locale (para projeto e aplicações).
Após fazer mudanças aos arquivos de mensagens você precisa compilá-los
com ``compilemessages`` para uso com o suporte gettext imbutido. Veja Capítulo
19 para detalhes.

--all
~~~~~

Use a opção ``--all`` ou ``-a`` para atualizar os arquivos de mensagens de todas
as linguagens disponiveis.

Exemplo de uso::

    django-admin.py makemessages --all

--extension
~~~~~~~~~~~

Use a opção ``--extension`` ou ``-e`` para especificar uma lista de extensões de arquivos
a serem examinadas (padrão: ".html").

Exemplo de uso::

    django-admin.py makemessages --locale=de --extension xhtml

Separe multiplas extensões com vírgulas ou use -e ou --extension multiplas vezes::

    django-admin.py makemessages --locale=de --extension=html,txt --extension xml

--locale
~~~~~~~~

Use a opção ``--locale`` ou ``-l`` para especificar o lacal do processo.

Exemplo de uso::

    django-admin.py makemessages --locale=br_PT

--domain
~~~~~~~~

Use a opção ``--domain`` ou ``-d`` para mudar o dominio dos arquivos de mensagens.
Atualmente suportados:

* ``django`` para todos arquivos ``*.py`` e ``*.html`` (padrão)
* ``djangojs`` para arquivos ``*.js``

.. _django-admin-reset:

reset <appname appname ...>
---------------------------

Executa o equivalente a ``sqlreset`` para o dado nome da aplicação(ões).

--noinput
~~~~~~~~~

Use a opção ``--noinput`` para suprimir todo prompt ao usuário, tais como
mensagens de confirmação "Are you sure?". Isto é útil caso ``django-admin.py``
esteja sendo executado como um script autônomo.

runfcgi [options]
-----------------

Inicia uma série de processos FastCGI apropriados para o uso com qualquer servidor Web que
dê suporte ao protocolo FastCGI. Veja o Capítulo 12 para detalhes. Requer o módulo Python
FastCGI de flup: http://www.saddi.com/software/flup/

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
