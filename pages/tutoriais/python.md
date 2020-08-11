Python no Windows
=================================

### Instalando o Python

[https://www.python.org/downloads/](https://www.python.org/downloads/)

Ao executar o instalador, selecione a opção **Add Python to PATH**.

### Instalando o PyCharm

o PyCharm será utilizada para criar os programas em Python.

[https://www.jetbrains.com/pt-br/pycharm/download/#section=windows](https://www.jetbrains.com/pt-br/pycharm/download/#section=windows)

Basta clicar em **Baixar** na sessão **Community**.

Ao instalar o PyCharm, marque em **Create Associations** a opção `.py`.

### Definindo variáveis de ambiente no Windows.

Vá para o seu usuário no Windows e digite na barra de endereço:

`C:\Users\jgmsa`

`jmsan` é o nome do meu usuário no Windows.

E digite: `AppData`

`C:\Users\jgmsa\AppData` 

Acesse o diretório:

`C:\Users\jgmsa\AppData\Local\Programs\Python\Python38-32\Scripts`

O `Python38-32 é a versão Python instalada no meu computador.

Apenas copie este caminho, pois ele será utilizado a seguir.

Vá para o painel de controle do Windows:

`Painel de Controle->Sistema e Segurança->Sistema->Configurações Avançadas do Sistema->Variáveis de Ambiente`

Há uma opção chamada `Variáveis do Sistema`, encontre a variável `Path`, clique em `Editar`, depois em `Novo` e cole a linha copiada acima.

Isso foi feito para adicionar os binários do Python a variável `Path`. 

Após fazer isso, clique em `OK` e depois clique em `Novo` para criar uma nova variável de ambiente e digite o nome abaixo:

Nome da variável `WORKON_HOME`
Valor da variável `%USERPROFILE%\Envs`

Basta clicar em `OK` e depois fechar tudo.

### Criando ambientes virtuais

Isto foi feito para criar ambientes virtuais no Python para deixar a instalação padrão do Python intocável, pois é uma prática muito boa em programação trabalhar com ambientais virtuais.

Em seguida, digite no seu terminal:

`pip --version`

Para saber se o pip está sendo reconhecido. Deve aparecer algo assim:

`pip 19.3.1 from /home/martins/.miniconda3/lib/python3.7/site-packages/pip (python 3.7)`

Depois digite os comandos no seu terminal do Windows:

`pip install virtualenv`
`pip install virtualenvwrapper-win`
`mkvirtualenv python`

O nome escolhido foi `python`, mas fica a sua escolha. Todo este processo foi feito para trabalhar com um ambiente virtual utilizando separado da instalação do Python que a forma correta de se trabalhar.

Note que o nome da sua linha de comando passa a se chamar `python` que foi o nome dado ao ambiente virtual.

Para remover um ambiente virtual, basta digitar:

`rmvirtualenv python`

Para sair de um ambiente virtual, basta digitar:

`deactivate`

Para entrar em um ambiente virtual, basta digitar:

`workon python`

A vantagem de fazer tudo isso, consiste em evitar conflitos nos programas Python utilizando versões diferentes, pois este procedimento mantém a mesma versão do Python mesmo que o seu sistema seja atualizado.

Feito todos os passos, agora começa a brincadeira com o PyCharm.