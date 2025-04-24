# Passos para criar ambiente virtual no Windows

## Instalação do terminal Git

Este terminal Git Bash é um terminal que pode ser utilizado no Windows e no Linux.

O terminal Git Bash está disponível em:

[https://git-scm.com/downloads](https://git-scm.com/downloads)

Os passos abaixo usam este terminal Git.

## Possibilidade 1: Usando venv

O `venv` é um módulo que já vem instalado com o Python. O `venv` é uma versão mais nova do `virtualenv`, mas o virtualenv ainda é muito utilizado. Funciona apenas no Python 3.3 e versões posteriores
   
```warning
Aviso: Antes de tudo, instalar o Python no Windows via Microsoft Store.
```

Abra o terminal do Git e digite Python e uma janela será aberta solicitando a instalação do Python no Windows. Após a instalação, o terminal do Git irá reconhecer o Python.

### Criar um ambiente virtual chamado venv

Este nome `venv` já é um padrão quando se cria ambiente virtual. Poderia ser qualquer outro nome, mas o padrão é `venv`.

Por padrão, o ambiente virtual é criado na pasta onde o comando é executado. Para criar o ambiente virtual, execute o comando abaixo:

Sintaxe:

```bash
python -m venv <nome_do_ambiente>
```

Exemplo: Criando o ambiente virtual chamado `venv`:

```bash
python -m venv venv
```

Onde: `-m` é o argumento que indica que o Python deve executar um módulo como um script e `venv` é o nome do módulo que será executado. O primeiro `venv` é o nome do módulo e o segundo `venv` é o nome do ambiente virtual que será criado.

### Habilitar o ambiente virtual

#### Via terminal do Linux

Para habilitar o ambiente virtual, execute o comando abaixo:

```bash
source venv/Scripts/activate
```

### Desativar o ambiente virtual

Para desativar o ambiente virtual, execute o comando abaixo:

```bash
deactivate
```

#### Via terminal do Windows

Para o caso de tentar ativar o ambiente virtual pelo Windows e aparecer a mensagem de erro abaixo:

```bash
.\venv\Scripts\activate : O arquivo C:\Users\guilherme.martins\guilherme\coisas\venv\Scripts\Activate.ps1 não pode ser carregado porque a execução de       
scripts foi desabilitada neste sistema. Para obter mais informações, consulte about_Execution_Policies em https://go.microsoft.com/fwlink/?LinkID=135170.
```

A solução está no link abaixo:

[Clique aqui](https://pt.stackoverflow.com/questions/220078/o-que-significa-o-erro-execu%C3%A7%C3%A3o-de-scripts-foi-desabilitada-neste-sistema)

Basta digitar o seguinte comando no terminal que está aberto no Windows:

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Feito isso, basta executar o comando abaixo para ativar o ambiente virtual:

```bash
.\venv\Scripts\activate
```

Para desativar o ambiente virtual, execute o seguinte comando:

```bash
deactivate
```

Em ambos os casos, tanto no Linux quanto no Windows, o terminal irá mostrar o nome do ambiente virtual que foi ativado. No caso do Windows, o nome do ambiente virtual será exibido entre parênteses, como no exemplo abaixo:

```bash
(venv) PS C:\Users\guilherme.martins\guilherme
```

## Possibilidade 2: Usando Anaconda

É a opção que mais gosto e uso. Isso é pessoal.

Acessar o site abaixo para realizar o download do Anaconda:

[https://www.anaconda.com/download](https://www.anaconda.com/download)


Preecncher os dados solicitados para realizar o downalod do executável. Ele tem um nome parecido com o que é mostrado abaixo:

```bash
Anaconda3-2024.10-1-Windows-x86_64.exe
```

A instalação demora um pouco.

Após a instalação finalizada, abra o terminal do Git Bash e digite:

```bash
conda --version
```

Quando fiz essa instalação, o comando acima retornou a versão do conda instalada:

```bash
conda 24.9.2
```

Depois, é só seguir os passos do 4 em diante para criar ambientes virtuais e instalar pacotes.

[https://guilherme.readthedocs.io/en/latest/pages/tutoriais/miniconda.html](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/miniconda.html)