Python para Windows
=================================

### Instalando o Python

[https://www.python.org/downloads/](https://www.python.org/downloads/)

Ao executar o instalador, selecione a opção **Add Python to PATH**.

### Instalando o PyCharm

o PyCharm é uma IDE (*Integrated Development Environment* ou Ambiente de Desenvolvimento Integrado) que será utilizada para criar os programas em Python.

[https://www.jetbrains.com/pt-br/pycharm/download/#section=windows](https://www.jetbrains.com/pt-br/pycharm/download/#section=windows)

Basta clicar em **Baixar** na sessão **Community**.

Ao instalar o PyCharm, marque em **Create Associations** a opção `.py`.

### Definindo variáveis de ambiente no Windows

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

### Iniciando em Python - Um pouco sobre PEP (Python Enhancement Proposals)

Uma boa prática é ler sobre as PEP 8:

[https://www.python.org/dev/peps](https://www.python.org/dev/peps)

A PEP 8 (link abaixo) é um guia de estilo de codificação em Python, ou seja, como escrever um programa de forma adequada:

[https://www.python.org/dev/peps/pep-0008](https://www.python.org/dev/peps/pep-0008)

Exemplos de PEP 8:

- Utilize sempre 4 espaços para identação em vez de TAB. O TAB pode ter configurações diferentes em computadores distintos.
- Utilize sempre letras minúsculas separadas por `_` para funções ou variáveis.
- Utilize sempre duas linhas em branco para separar funções e definições de classe com duas linhas em branco.
- Métodos dentro de uma classe devem separados por uma única linha em branco.
- Imports devem ser feitos em linhas separadas.
  - `import sys`
  - `import os`
- Os imports são sempre declarados no topo do script.

### Iniciando em Python

Utilitários Python (`dir` e `help`) para auxiliar na programação.

`dir`: Apresenta todos os atributos/propriedades e funções/métodos disponíveis
para determinado tipo de dado ou variável.

dir(tipo de dado ou variável)

- Exemplo: Com o Python ativo, digitar no terminal o comando abaixo:

`>>> dir("Meteorologia")`

![](../../images/python_tutorial/fig01.png)

Ao digitar este comando serão mostradas todas as opções que podem ser utilizadas com o tipo `"Meteorologia"`.

Por exemplo, deseja-se saber o que um determinado comando faz, como no exemplo, para isso, utiliza-se o help.

`>>> "Meteorologia".upper`

`help`: Apresenta a documentação/como utilizar os atributos/propriedades e funções/métodos disponíveis para determinado tipo de dado ou variável.

`>>> help("Meteorologia".upper)`

![](../../images/python_tutorial/fig02.png)

Será mostrado o que o comando faz.

Ao digitar o comando abaixo:

`>>> "Meteorologia".upper()`

![](../../images/python_tutorial/fig03.png)

Toda a string foi convertida para o formato maiúsculo.

### Sobre variáveis em Python

Existem dois tipos:

- Váriáveis globais:
    - Variáveis globais são reconhecidas, ou seja, seu escopo compreende todo o programa.
    - Exemplo:
    ```python
    num = 2
    print(num)
    print(type(num))
    ````
- Variáveis locais:
    - Variáveis locais são reconhecidas apenas no bloco onde foram declaradas, ou seja, seu escopo está limitado ao bloco onde foi declarada.
    - Exemplo:
    ```python
    numero = 2
    if numero > 10:
        novo = numero + 10

    print(novo)
    ```
    - Será gerado erro porque a variável `novo` faz parte do contexto da estrutura condicional `if`.

- Para declarar variáveis em Python, utiliza-se:
    - `nome_da_variavel = valor_da_variavel`

O `Python` é uma liguagem de `tipagem dinâmica`. Isso siginifica que ao declarar uma variável, não há necessidade de informar o tipo da variável.

### Estrutura condicional

- Estrutura if:

```python
temperatura = 30
if temperatura < 40:
    print("Temperatura menor que 40 graus Celsius")
```

- Estrutura if-else:

```python
temperatura = 45
if temperatura < 40:
    print("Temperatura menor que 40 graus Celsius") # Sempre utilizar quatro espaços. Caso contrários será retornado erro.
else:
    print("Temperatura maior que 40 graus Celsius") # Sempre utilizar quatro espaços. Caso contrários será retornado erro.
```

- Estrutura if-elif-else:

```python
temperatura = 50
if temperatura < 30:
    print("Temperatura menor que 30 graus Celsius")
elif temperatura == 50:
    print("Temperatura igual a 50 graus Celsius")
else:
    print("Temperatura maior que 30 graus Celsius")
```

É possível utilizar vários `elif` que dependerá da condição.

### Estrutura lógica

- Estruturas lógicas: `and` (e), `or` (ou), `not` (não) e `is` (é).
- Operadores unários, isto é, dependem apenas de um valor:
    - `not`
- Operadores binários:
    - `and`, `or` e `is`
- Regras de funcionamento:
  - Para o `and`, ambos os valores devem ser `True`.
  - Para o `or`, um ou outro valor precisa ser `True`.
  - Para o `not`, o valor do booleano (`True` ou `False`) é invertido, ou seja, se for `True`, vira `False`, e vice-versa.
  - Para o `is`, o valor é comparado com outro valor.

- Exemplo de uso do `and`:

```python
umidade_relativa = 80
temperatura = 20

if umidade_relativa <= 30 and temperatura >= 40:
    print("Condição perigosa")
else:
    print("Condição favorável")
```

- Exemplo de uso do `or`:
```python
umidade_relativa = 80
temperatura = 45

if umidade_relativa <= 30 or temperatura >= 40:
    print("Condição perigosa")
else:
    print("Condição favorável")
```

- Exemplo de uso do `not`:
```python
umidade_relativa = 80

if not umidade_relativa <= 30:
    print("Umidade relativa alta")
else:
    print("Umidade relativa baixa")
```

- Exemplo de uso do `is`:
```python
umidade_relativa = 80

if (umidade_relativa <= 30) is False:
    print("Umidade relativa alta")
else:
    print("Umidade relativa baixa")
```

O trecho `(umidade_relativa <= 30)` é `False`, logo `False is False`? Verdade, por isso, `print("Umidade relativa alta")`

### Estrutura de repetição

#### Loop for

- Loop: É uma estrutura de repetição.
- Utilizamos loops para iterar sobre sequências ou sobre valores iteráveis.
- Loop `for` (para).
- `for`: É uma dessas estruturas.
- Exemplo1:
```python
nome = "Meteorologia"

for letra in nome:
    print(letra)
```

- Exemplo2:
```python
for numero in range(1, 10): # O último número é exclusivo.
    print(numero)
```

#### Loop while

- O bloco do `while` será repetido enquando a `expressão_booleana` for verdadeira. Expressão booleana é toda expressão onde o resultado é `True` (verdadeiro) ou `False` (falso).
- Em um loop while é importante que cuidemos do critério de parada para não causar um loop infinito.

```python
Exemplo1: Compara o valor 10 com o 5.

num = 10
print(num < 5) # False
```

```python
Exemplo2: Imprime os valores de 1 a 9, lembrando que o último valor é exclusivo.

numero = 1

while numero < 10:
    print(numero)
    numero = numero + 1
```

```python
Exemplo3: Enquanto o usuário não digitar sim, o loop será executado.

resposta = ''

while resposta != 'sim':
    resposta = input("Já acabou Jéssica?")
```

#### break

- Utilização do `break` para sair de loops de maneira projetada.
- Exemplo1:
```python
for numero in range(1, 11):
    if numero == 6:
        break
    else:
        print(numero)
print('Sai do loop') # print está fora do bloco for.
```

- Exemplo2:
```python
while True:
    comando = input("Digite 'sair' para sair")
    if comando == 'sair':
        break
```