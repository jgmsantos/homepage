Miniconda
============

## 1. Introdução

O miniconda ajuda bastante na hora de instalar um programa porque ele resolve todas as dependências de forma que o usuário não precise ficar quebrando a cabeça na hora da instalação. 

A criação de um ambiente virtual facilita porque caso seja feita alguma instalação de programa e ele passe por alguma instabilidade, basta remover o ambiente virtual e criar um novo sem danificar o sistema operacional.

## 2. Instalação do miniconda no Linux por meio do site oficial:

[https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

## 3. Baixar o instalador

+ Digitar no seu terminal Linux o comando:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

+ Executar o instalador

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

Basta aceitar todas as opções que vão aparecendo no processo de instalação.

Após a instalação é necessário reiniciar o terminal do Linux para que as mudanças sejam aplicadas.

Após reiniciar o terminal, ele ficará assim:

```bash
(base) guilherme@DESKTOP-LD7TCRV:~$ 
```

Note o nome `(base)` na frente do nome do usuário.

## 4. Para atualizar o conda

```bash
conda update -n base -c defaults conda
```

## 5. Criar um novo ambiente virtual

```bash
conda create --name <nome_ambiente>
```

Exemplo: Criação do ambiente virtual chamado `inpe`.

```bash
conda create --name inpe
```
## 6. Ver a lista de programas instalados

```bash
conda list
```

## 7. Desativar um ambiente virtual

```bash
conda deactivate
```

Ao fazer isso, o usuário sairá do ambiente virtual corrente e mudará para o ambiente virtual (base).

## 8. Ativar um ambiente

```bash
conda activate <nome_ambiente>
```

Exemplo: 

```bash
conda activate inpe
```
Antes o ambiente era o `(base)`.

```bash
(base) guilherme@DESKTOP-LD7TCRV:~$
```

Após o comando, ficará assim:

```bash
(inpe) guilherme@DESKTOP-LD7TCRV:~$
```

## 9. Remover um ambiente

```bash
conda env remove --name <nome_ambiente>
```

Exemplo: 

```bash
conda env remove --name inpe
```

## 10. Instalação de programas

Caso seja necessário instalar um programa, mas não se sabe como fazê-lo, basta pesquisar no google, por exemplo, `cdo instalação conda`. Ao clicar na imagem abaixo:

OBS: A instalação é feita considerando sempre a versão mais recente do programa.

![](../../images/miniconda/fig01.JPG)

Aparecerá a imagem abaixo, basta copiar e colar a primeira linha da imagem no terminal do Linux,  ou seja:

```bash
conda install -c conda-forge cdo
```

![](../../images/miniconda/fig02.JPG)


### 10.1 Instalação do NCL

```bash
conda install -c conda-forge ncl
```

### 10.2 Instalação do gdal

```bash
conda install -c conda-forge gdal
```

### 10.3 Instalação do CDO

```bash
conda install -c conda-forge cdo
```

### 10.4 Instalação do imagemagick

```bash
conda install -c conda-forge imagemagick
```

### 10.5 Instalação do ncview

```bash
conda install -c conda-forge ncview
```
### 10.6 Instalação do htop

```bash
conda install -c conda-forge htop
```

### 10.7 Instalação do parallel

```bash
conda install -c conda-forge parallel
```

### 10.8 Instalação do parallel

```bash
conda install -c conda-forge nco
```

### 10.9 Instalação do wgrib2

```bash
conda install -c conda-forge wgrib2
```

Ao executar o wgrib2 e aparecer o erro abaixo:

```bash
wgrib2: error while loading shared libraries: libjasper.so.1: cannot open shared object file: No such file or directory
```

Basta criar um link simbólico de acordo com o comando abaixo. Tudo isso é uma linha. Altere os caminhos de acordo com o seu usuário (`queimadas`) e ambiente virtual (`risco_fogo`):

```bash
ln -s /home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.4 /home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.1
```

Onde: `/home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.4` representa a biblioteca que está no computador.

E `/home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.1` representa o link simbólico que será a biblioteca a ser criada no seu computador, a que foi mostrada no erro.

Outro possível erro que pode aparecer:

```bash
wgrib2: error while loading shared libraries: libnetcdf.so.13: cannot open shared object file: No such file or directory
```

Basta criar um link simbólico de acordo com o comando abaixo. Tudo isso é uma linha. Altere os caminhos de acordo com o seu usuário (`queimadas`) e ambiente virtual (`risco_fogo`):

```bash
ln -s /home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.19 /home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.13
```

Onde: `/home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.19` representa a biblioteca que está no computador.

E `/home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.13` representa o link simbólico que será a biblioteca criada, a que foi mostrada no erro.

## 10.10. Instalação de uma versão especifica de um programa 

```bash
conda install <nome_programa>=<versão_programa>
```

Exemplo: Instalar a versão do cdo 1.9.9.

```bash
conda install cdo=1.9.9
```

## 11. Para atualizar um pacote:

```bash
conda update <nome_pacote>
```
Exemplo:

```bash
conda update cdo
```
## 12. Desinstalar pacote

```bash
conda remove <nome_pacote>
````

Caso não funcione, pode-se usar o `--force` para forçar a desinstalação:

```bash
conda remove --force <nome_pacote>
```

Exemplo: Desinstalar o cdo.

```bash
conda remove cdo
```
E para forçar a desinstalação do CDO caso o comando acima não funcione:

```bash
conda remove --force cdo
```

## 13. Links interessantes
+ [https://guilherme.readthedocs.io/en/latest](https://guilherme.readthedocs.io/en/latest)
+ [https://guilherme.readthedocs.io/en/latest/pages/tutoriais/index.html](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/index.html)
+ [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

## 14. Rede Sociais
+ [https://github.com/jgmsantos/Scripts](https://github.com/jgmsantos/Scripts)
+ [https://www.linkedin.com/in/guilherme-martins-7239241a5](https://www.linkedin.com/in/guilherme-martins-7239241a5)
+ [https://www.instagram.com/jgmsantos](https://www.instagram.com/jgmsantos)
+ [https://www.facebook.com/guilherme.martins.77398](https://www.facebook.com/guilherme.martins.77398)
+ [https://www.researchgate.net/profile/Guilherme-Martins-18](https://www.researchgate.net/profile/Guilherme-Martins-18)

