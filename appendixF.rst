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

Inicia um servidor Web leve de desenvolvimento na máquina local. Por padrão,
o servidor é executado na porta 8000 com o endereço de IP 127.0.0.1. Você pode fornecer
um endereço de IP e um número de porta explicitamente.

Se você executar este script como um usuário com restrições normais (recomendado), você
pode não ter acesso a começar uma porta em um número de porta baixo. Números de porta baixos
são reservados ao superusuário (raiz).

NÂO USE ESTE SERVIDOR EM UM AMBIENTE DE PRODUÇÂO. Ele não passou por auditorias
de segurança ou testes de desempenho. (E é assim que ele vai ficar. Nós estamos
no negócio de fazer frameworks para Web, não servidores Web, assim melhorar este
servidor para que ele seja capaz de lidar com um ambiente de produção está fora
do âmbito do Django.)

O servidor de desenvolvimento recarrega automaticamente o código Python para cada solicitação,
caso seja necessário. Você não precisa reiniciar o servidor para mudanças de código entrarem em efeito.

Quando você iniciar o servidor, e a cada vez que você alterar o código Python enquanto
o servidor estiver executando, o servidor irá validar todos os seus modelos instalados. (Veja
o comando ``validate`` abaixo.) Se o validador encontrar erros, ele irá imprimi-los
para a saída padrão, mas não vai parar o servidor.

Você pode executar quantos servidores você quiser, desde que eles estejam em portas diferentes.
Apenas execute ``django-admin.py runserver`` mais de uma vez.

Note que o endereço IP padrão, 127.0.0.1, Não é acessível a partir de outras
máquinas na sua rede. Para fazer seu servidor de desenvolvimento visível a
outras máquinas na sua rede, use seu próprio endereço IP (e.g. ``192.168.2.1``) ou
``0.0.0.0`` (O qual você pode usar se você não sabe qual é o endereço IP da
sua rede).

Use a opção ``--adminmedia`` para indicar ao Django onde encontrar vários arquivos
CSS e JavaScript para a interface de administração do Django. Normalmente, o servidor
de desenvolvimento serve estes arquivos da árvore fonte do Django magicamente, mas você
gostaria de usar isto se você fizesse alguma mudança nestes arquivos para o seu site.

Exemplo de uso::

    django-admin.py runserver --adminmedia=/tmp/new-admin-style/

Use a opção ``--noreload`` para desativer o auto-reloader. Isto significa
que qualquer mudança que você fizer no código Python enquanto o servidor estiver em execução
*não* tomarão efeito se os módulos Python em particular já foram carregados na memória.

Exemplo de uso::

    django-admin.py runserver --noreload

Exemplos usando diferentes portas e endereços
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Porta 8000 no endereço IP 127.0.0.1::

	django-admin.py runserver

Porta 8000 no endereço IP 1.2.3.4::

	django-admin.py runserver 1.2.3.4:8000

Porta 7000 no endereço IP 127.0.0.1::

    django-admin.py runserver 7000

Porta 7000 no endereço IP 1.2.3.4::

    django-admin.py runserver 1.2.3.4:7000

Servindo arquivos estáticos com o servidor de desenvolvimento
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Por padrão, o servidor de desenvolvimento não serve nenhum arquivo estático para seu site
(assim como arquivos CSS , imagens, arquivos dentro de ``MEDIA_URL`` e assim por diante).

shell
-----

Inicia o interpretador interativo do Python.

Django usará IPython (http://ipython.scipy.org/), se estiver instalado. Se você 
tem IPython instalado e quer forçar o uso do interpretador "simples" do Python,
use a opção ``--plain``, dessa maneira::

    django-admin.py shell --plain

sql <appname appname ...>
-------------------------

Imprime as declerações CREATE TABLE SQL para o(s) nome(s) do aplicativo(s) dado(s).

sqlall <appname appname ...>
----------------------------

Imprime as declerações CREATE TABLE e initial-data SQL para o(s) nome(s) do aplicativo(s) dado(s).

COnsulte a descrição de ``sqlcustom`` para uma explicação de como
especificar dados iniciais.

sqlclear <appname appname ...>
------------------------------

Imprime as declerações DROP TABLE SQL para o(s) nome(s) do aplicativo(s) dado(s).

sqlcustom <appname appname ...>
-------------------------------

Prints the custom SQL statements for the given app name(s).

Para cada modelo em cada aplicativo especificado, este comando procura pelo arquivo
``<appname>/sql/<modelname>.sql``, onde ``<appname>`` é o nome do aplicativo dado e
``<modelname>`` é o nome do modelo em letras minúsculas. Por exemplo, se você tem um
aplicativo ``news`` o qual inclui um modelo ``Story``, ``sqlcustom`` vai tentar ler
um arquivo ``news/sql/story.sql`` e anexá-lo à saída deste comando.

Cada um dos arquivos SQL, caso fornecidos, devem conter SQL válido. Os arquivos SQL
são alimentados à base de dados após todas as declarações de criação de tebelas dos
modelos tenham sido executadas. Use este atalho do SQL para fazer modificações em tabelas,
ou inserir qualquer função SQL no banco de dados.

Note que a ordem em que os arquivos SQL são processados é indefinida.

sqlflush
--------

Imprime as declarações SQL que seriam executadas para o comando `flush`_.

sqlindexes <appname appname ...>
--------------------------------

Imprime as declerações SQL CREATE INDEX para o nome do aplicativo(s) fornecido.

sqlreset <appname appname ...>
------------------------------

Imprime o DROP TABLE SQL, depois o CREATE TABLE SQL, para o nome do aplicativo(s) fornecido.

sqlsequencereset <appname appname ...>
--------------------------------------

Imprime as declarações SQL para redefinição de sequências para o nome do aplicativo(s) fornecido.

startapp <appname>
------------------

Cria uma estrutura de diretório para um aplicativo Django para o nome do aplicativo dado no
diretório atual.

startproject <projectname>
--------------------------

Cria uma estrutura de diretório para um projeto Django para o nome do projeto dado no
diretório atual.

Este comando é disabilitado quando a opção ``--settings`` para
``django-admin.py`` for usada, ou quando a variável de ambiente
``DJANGO_SETTINGS_MODULE`` for ativada. Para reabilitá-lo nestas
situações, omita a opção ``--settings`` ou desative a variável
``DJANGO_SETTINGS_MODULE``.

syncdb
------

Cria todas as tabelas de banco de dados para todos os aplicativos em ``INSTALLED_APPS`` cujas tabelas
ainda não foram criadas.

Use este comando quando você adicionar novos aplicativos em seu projeto e queira
instalá-los no banco de dados. Isto inclui quaisquer aplicativos fornecidos com o Django que
podem estar em ``INSTALLED_APPS`` por padrão. Quando você iniciar um novo projeto, execute
este comando para instalar os aplicativos padrão.

.. advertência:: Syncdb não alterará tabelas existentes

   ``syncdb`` apenas criará tabelas para os modelos que ainda não foram
   instalados. Ele nunca vai emitir declarações ``ALTER TABLE`` para coincidir
   alterações feitas em uma classe modelo após a sua instalação. Mudanças em classes modelo
   e esquemas de bancos de dados, muitas vezes envolvem alguma forma de ambiguidade and, nestes
   casos, Django teria que adivinhar as mudanças corretas a fazer. Existe um risco
   que dados críticos podem ser perdidos durante o processo.

   Se você fez mudanças em um modelo e quiser alterar as tabelas do banco de dados
   para coincidir a informação, use o comando ``sql`` para exibir a nova estrutura SQL e
   compará-la com seu esquema de tabelas existente para resolver as diferenças.

Se você está instalando a aplicação ``django.contrib.auth``, ``syncdb``
vai lhe dar a opção de criar um superusuário imediatamente.

``syncdb`` também vai procurar e instalar qualquer fixture chamada ``initial_data``
com uma extensão apropriada (e.g. ``json`` or ``xml``).Veja a documentação
para ``loaddata`` para obter detalhes sobre a especificação de arquivos de dados
fixture.

--noinput
~~~~~~~~~

Use a opção ``--noinput`` para suprimir todo prompt ao usuário, tais como
mensagens de confirmação "Are you sure?". Isto é útil se ``django-admin.py``
está sendo executado como um script autônomo.

test
----

Roda teste para todos os modelos instalados. Consulte a documentação do Djando para
mais informações sobre testes.

--noinput
~~~~~~~~~

Use a opção ``--noinput`` para suprimir todo prompt ao usuário, tais como
mensagens de confirmação "Are you sure?". Isto é útil se ``django-admin.py``
está sendo executado como um script autônomo.

testserver <fixture fixture ...>
--------------------------------

Executa um servidor de desenvolvimento Django (assim como ``runserver``) usando dados da
fixture(s) dadas.

Para mais informações, Consulte a documentaçãos do Django.

validate
--------

Valida todos os modelos instalados (de acordo com as configurações ``INSTALLED_APPS``)
e imprime os erros de validação para a saída padrão.

Opções padrão
===============

Apesar de alguns subcomandos permitirem suas próprias opções customizadas, todos subcomandos
permitem as seguintes opções:

--pythonpath
------------

Exemplo de uso::

    django-admin.py syncdb --pythonpath='/home/djangoprojects/myproject'

Adiciona o caminho de sistema dado ao caminho de busca de importção do Python. Caso isto
não seja providenciado, ``django-admin.py`` usará a variável de ambiente ``PYTHONPATH``.

Note que esta opção é desnecessária em ``manage.py``, porque ele cuida de definir o caminho
do Python para você.

--settings
----------

Exemplo de uso::

    django-admin.py syncdb --settings=mysite.settings

Especifica explicitamente o módulo de configurações a ser usado. O módulo de configurações
deve estar em sintaxe Python package, e.g. ``mysite.settings``. Se isto não for providenciado,
``django-admin.py`` usará a variável de ambiente ``DJANGO_SETTINGS_MODULE``.

Note que esta opção é desnecessária em ``manage.py``, porque ele usa
``settings.py`` do projeto atual por padrão.

--traceback
-----------

Exemplo de uso::

    django-admin.py syncdb --traceback

Por padrão, ``django-admin.py`` irá mostrar uma mensagen de erro simples quando
um erro ocorre. Se você especificar ``--traceback``, ``django-admin.py``  irá
imprimir um rastreamento de pilha completo sempre que uma exeção for levantada.

--verbosity
-----------

Exemplo de uso::

    django-admin.py syncdb --verbosity 2

Use ``--verbosity`` para especificar a quantidade de notificações e informação de debuging
que ``django-admin.py`` deve imprimir no console.

* ``0`` significa nenhuma saída.
* ``1`` significa saída norma (padrão).
* ``2`` significa saída verbose.

Sutilezas extras
==============

Colorindo a sintaxe
---------------

O comando ``django-admin.py`` / ``manage.py`` que imprime saída SQL para
saída padrão, usará saída codificada em cores se o seu terminal tem suporte para
saída ANSI colorida. Ele não vai usar o código de cores se você estiver alimentando
a saída do comando para outro programa.

Bash completion
---------------

Se você usar o Bash shell, considere instalar o script do Django bash completion,
o qual está dentro de ``extras/django_bash_completion`` na distribuição do Django.
Ele ativa tab-completion dos comandos ``django-admin.py`` e ``manage.py``,
e então você pode, por exemplo...

* Digitar ``django-admin.py``.
* Apertar [TAB] para ver todas as opções disponíveis.
* Digitar ``sql``, depois apertar [TAB], para ver todas as opções disponíveis cujos nomes
  começam com ``sql``.
