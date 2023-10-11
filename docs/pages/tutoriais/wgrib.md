Instalação do wgrib
========================

Acessar o site abaixo:

[https://www.cpc.ncep.noaa.gov/products/wesley/wgrib.html](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib.html)

E ir para a item **Source code:**

Em seguida, clicar em

**wgrib.tar See above for source code. (For people who want to read or re-use the source code.)**

Ou,

no terminal do Linux, digitar o comando abaixo para realizar o download do arquivo via terminal:

```
wget https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib/wgrib.tar
```

Criar o diretório ```local``` no ```/home/ubuntu```

Essa instalação é feita quando o **usuário não tem permissão** de instalar programas na máquina.

```
mkdir local
```

Criar o diretório ```wgrib``` dentro do diretório ```local```

```
cd /home/ubuntu/local
```

```
mkdir wgrib
```

Instalar o gcc

```
sudo apt install gcc
```

Instalar o ```make```

```
sudo apt install make
```

Mover o arquivo ```wgrib.tar``` para o diretório ```wgrib```

Lembrando que ```wgrib.tar``` é o arquivo que foi baixado pelo ```wget```.

```
mv wgrib.tar /home/ubuntu/local/wgrib
```

Descompactar o arquivo ```wgrib.tar``` dentro do diretório ```wgrib```

```
tar -xvf wgrib.tar
```

Digitar o comando ```make``` dentro do diretório ```wgrib```

```
make
```

Ao digitar este comando, será criado o executável ```wgrib``` dentro de:

```
/home/ubuntu/local/wgrib
```

Copiar o executável criado para o diratório bin

Dentro do diretório wgrib, copiar o executável criado (```wgrib```) para:

```
/home/ubuntu/anaconda3/envs/meteo/bin/
```

Por meio do comando:

```
cp wgrib /home/ubuntu/anaconda3/envs/meteo/bin/
```

Links adicionais:

* [https://www.cpc.ncep.noaa.gov/products/wesley/wgrib.html](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib.html)
* [https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib/grib2ieee.txt](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib/grib2ieee.txt)
* [https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib)