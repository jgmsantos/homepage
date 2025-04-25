# Criar ambiente virtual no Windows

## Instalação usando Anaconda

Acesse o link abaixo para baixar o Anaconda:

[https://www.anaconda.com](https://www.anaconda.com/)

No canto superior direito, clique em `Free Download`. Insira seu e-mail e clique em `Submit`. Você será direcionado para uma nova página que contém os executáveis para download. 

Há duas opções: 

* Anaconda Installers
* Miniconda Installers 
 
A diferença entre os dois é que o `Anaconda` já vem com uma série de pacotes pré-instalados (é mais pesado), enquanto o `Miniconda` é uma versão mais leve e permite que você instale apenas os pacotes que precisa. Para a maioria dos usuários, o Anaconda é a melhor opção.

Neste exemplo, será utilizado o `Miniconda Installers` para Windows. Basta clicar em `64-Bit Graphical Installer` para realizar o download do executável. Na pasta `Downloads`, clique duas vezes no arquivo `Miniconda3-latest-Windows-x86_64.exe` para iniciar a instalação. Basta seguir as instruções do instalador.

No menu Iniciar do Windows deve ter uma pasta assim:

`Anaconda (miniconda3)`

Que contém os dois promptos de comando:
* Anaconda Powershell Prompt
* Anaconda Prompt

Ambos podem ser utilizados para criar ambientes virtuais e instalar/desinstalar pacotes. 

## Instalação de pacotes

Eu particularmente gosto de usar o `Anaconda Prompt`, pois ele é mais leve.

Ao abrir o `Anaconda Prompt`, você verá uma tela semelhante a esta:

```bash
(base) C:\Users\<seu_usuario> 
```

Terá o nome `(base)` do lado esquerdo, isso quer dizer que você está no ambiente base do Anaconda. 

NUNCA use ou instale pacotes nesse ambiente, SEMPRE crie um novo ambiente virtual para instalar/desinstalar pacotes.

Para criar um ambiente virtual e instalar pacotes, siga os passos abaixo:

[https://guilherme.readthedocs.io/en/latest/pages/tutoriais/miniconda.html#para-atualizar-o-conda](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/miniconda.html#para-atualizar-o-conda)
