Instalação do wgrib2
========================

### Opção 1: Instalação na mão

Vá para o diretório `Downloads` e digite: 

`wget ftp://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz`


Em seguida, digite o comando abaixo para descompactar o arquivo `wgrib2.tgz`: 

`tar -zxvf wgrib2.tgz`

Entre no diretório `grib2`: 

`cd grib2`

Dentro do diretório `grib2` digite as linhas abaixo

```
export CC=gcc
export FC=gfortran
make
```

Será criado o diretório `wgrib2`, entre nele e copie o executável `wgrib2` para o diretório do seu interesse.

Adicione o caminho onde está o executável `wgrib2` na sua variável de ambiente `PATH` que está no arquivo `.bashrc`. Não esqueça de atualizar o `.bashrc` digitando `source .bashrc`. Isso é feito no seu diretório `home`.

+ Exemplos de uso do wgrib2 podem ser encontrados em:

	+ [http://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/netcdf.html](http://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/netcdf.html)

	+ [http://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2](http://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2)

### Opção 2: Instalação via conda

```
conda install -c conda-forge wgrib2
```

Ao executar o wgrib2 e aparecer o erro abaixo:

```
wgrib2: error while loading shared libraries: libjasper.so.1: cannot open shared object file: No such file or directory
```

Basta criar um link simbólico de acordo com o comando abaixo. Tudo isso é uma linha. Altere os caminhos de acordo com o seu usuário (`queimadas`) e ambiente virtual (`risco_fogo`):

```
ln -s /home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.4 /home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.1
```

Onde: `/home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.4` representa a biblioteca que está no computador.

E `/home/queimadas/miniconda3/envs/risco_fogo/lib/libjasper.so.1` representa o link simbólico que será a biblioteca a ser criada no seu computador, a que foi mostrada no erro.

Outro possível erro que pode aparecer:

```
wgrib2: error while loading shared libraries: libnetcdf.so.13: cannot open shared object file: No such file or directory
```

Basta criar um link simbólico de acordo com o comando abaixo. Tudo isso é uma linha. Altere os caminhos de acordo com o seu usuário (`queimadas`) e ambiente virtual (`risco_fogo`):

```
ln -s /home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.19 /home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.13
```

Onde: `/home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.19` representa a biblioteca que está no computador.

E `/home/queimadas/miniconda3/envs/risco_fogo/lib/libnetcdf.so.13` representa o link simbólico que será a biblioteca criada, a que foi mostrada no erro.

### Exemplos de uso do wgrib2

#### Visualizar informações sobre o arquivo:

`wgrib2 input.grb2`

#### Converter grib2 para NetCDF:

`wgrib2 gfs.t18z.pgrb2.0p25.anl -netcdf output.nc`

#### Selecionar uma variável `(‘:TMP:’)` de interesse e salvar no formato NetCDF:

` wgrib2 -match ':TMP:' gfs.t18z.pgrb2.0p25.anl -netcdf output.nc`

#### Selecionar uma variável de interesse e recortar o dado em um domínio particular:

+ Link: [https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/lola.html](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/lola.html)

  Sintaxe: `wgrib2 -lola LonSW:#lon:dlon LatSW:#lat:dlat nome_do_arquivo formato_da_saída`
  
Onde:

 + `LonSW` é o valor da longitude oeste do seu domínio (sempre no formato 0° a 360°)
 + `#lon` é o Número de pontos de longitude desejados
 + `dlon` é a resolução espacial em graus do dado
 + `LatSW` é o valor da latitude sul do seu domínio (sempre no formato -90 a 90)
 + `#lat` é o número de pontos de latitude desejados
 + `dlat` é a resolução espacial em graus do dado
 + O `nome_do_arquivo` é o nome do arquivo a ser gerado 
 + `formato_da_saída` e o formato de saída do dado, pode ser: `bin` = binary, `text` = simple "text" format, `one value per line`, `spread` = spread-sheet format, `latitude`, `longitude` and value of each grid point e `grib` = grib2
 
 + Exemplo:

`wgrib2 -match '(:TMP:30-0)' gfs.t18z.pgrb2.0p25.anl -lola 240:349:0.25 -56:357:0.25 output.grb grib`

Onde: 

  + `240` é o ponto de longitude oeste do domínio
  + `349` é o número de pontos que se deseja
  + `0.25` é a resolução espacial em graus 
  + `-56` é o ponto de latitude sul do domínio
  + `357` é o número de pontos que se de deseja 
  + `0.25` é a resolução em graus.
  + O domínio de interesse é: `Lon.: 120°W - 33°W` e `Lat.: 56°S - 33°N`
  + a resolução espacial do dado é de `0.25°`
  + 120°W = 360° - 120° = 240° => Longitude Oeste
  + 33°W = 360° - 33° = 327° => Longitude Leste
  + 327° - 240° = 87°/0.25° = 348 + 1 = 349, que corresponde ao número de pontos de longitude desejado
  + 56°S + 33°N = 89°/0.25° = 356 + 1 = 357, que corresponde ao número de pontos de latitude desejado

### Outras aplicações

##### Arquivo de exemplo

Para baixar os dois arquivos para a variável TMP (temperatura), basta executar a linha de comando abaixo, não esquecendo de alterar a data ```20230322```.

* Previsão de TMP: f001.

```
curl "https://nomads.ncep.noaa.gov/cgi-bin/filter_gfs_0p25.pl?file=gfs.t00z.pgrb2.0p25.f001&all_lev=on&var_TMP=on&leftlon=0&rightlon=360&toplat=90&bottomlat=-90&dir=%2Fgfs.20230322%2F00%2Fatmos" --output tmp.f001.grib2
```

O comando acima gera o arquivo: ```tmp.f001.grib2```

* Análise (anl):

curl "https://nomads.ncep.noaa.gov/cgi-bin/filter_gfs_0p25.pl?file=gfs.t00z.pgrb2.0p25.anl&all_lev=on&var_TMP=on&leftlon=0&rightlon=360&toplat=90&bottomlat=-90&dir=%2Fgfs.20230322%2F00%2Fatmos" --output tmp.anl.grib2

O comando acima gera o arquivo: ```tmp.anl.grib2```

* Previsão de u e v: f001.

```
curl "https://nomads.ncep.noaa.gov/cgi-bin/filter_gfs_0p25.pl?file=gfs.t00z.pgrb2.0p25.f001&lev_850_mb=on&var_UGRD=on&var_VGRD=on&leftlon=0&rightlon=360&toplat=90&bottomlat=-90&dir=%2Fgfs.20230322%2F00%2Fatmos" --output uv.grib2
```

O comando acima gera o arquivo: ```uv.grib2```

##### Mostrar o domínio espacial do arquivo

```wgrib2 -domain tmp.f001.grib2```

##### Mostar o nome da variável do arquivo

```wgrib2 -ext_name tmp.f001.grib2```


##### Mostrar informações sobre o domínio espacial do arquivo

```wgrib2 -grid tmp.f001.grib2```

##### Mostrar os níveis verticais do arquivo:

```wgrib2 -lev tmp.f001.grib2```

##### Mostrar o valor da variável em uma determinada longitude/latitude

A conversão será sempre: 

```wgrib2 -lon <longitude> <latitude> <arquivo.grib2>```

Exemplo:

```wgrib2 -lon -60 -30 tmp.f001.grib2```

##### Mostrar o valor máximo

```wgrib2 -max tmp.f001.grib2```

##### Mostra o valor mínimo

```wgrib2 -min tmp.f001.grib2```

##### Mostrar o número de pontos de grade

```wgrib2 -nxny tmp.f001.grib2```

##### Mostrar as informações do arquivo

```wgrib2 -s tmp.f001.grib2```

Ou, mais completa:

```wgrib2 -S tmp.f001.grib2```

##### Mostrar estatística do arquivo

```wgrib2 -stats tmp.f001.grib2 ```

##### Mostrar a saída diagnóstica

```wgrib2 -V tmp.f001.grib2```

##### Mostrar o nome da variável

```wgrib2 -var tmp.f001.grib2```

##### Verbose

```wgrib2 -v0 tmp.f001.grib2```

**Opções**: v, v0, v1, ..., v99

##### Gerar um arquivo csv

Geração do arquivo de TMP paRA 1000mb:

```wgrib2 -match ":TMP:1000 mb:" tmp.f001.grib2 -grib tmp.1000mb.f001.grib2```

Geração do arquivo no formato ".csv".

```wgrib2 tmp.1000mb.f001.grib2 -csv saida.csv```

Outra possibilidade:

```wgrib2 tmp.1000mb.f001.grib2 -csv_long saida.csv```

##### Calcular a direção do vento (em graus) a partir de u e de v

Neste exemplo, as componentes do vento são extraídas no nível de 850 mb para calcular a direção do vento.

```wgrib2 -match ':(UGRD|VGRD):850 mb' gfs_f001_20230414.grib2 -wind_dir dir850hpa.grib2```

Onde: 

**gfs_f001_20230414.grib2**: é o seu arquivo grib2

**dir850hpa.grib2** : é o arquivo que será gerado com a direção do vento. Altere o nome de acordo com a sua necessidade.

##### Calcular a velocidade do vento (em m/s) a partir de u e de v

Neste exemplo, as componentes do vento são extraídas no nível de 850 mb para calcular a velocidade do vento.

```wgrib2 -match ':(UGRD|VGRD):850 mb' gfs_f001_20230414.grib2 -wind_speed vel850hpa.grib2```

Onde: 

**gfs_f001_20230414.grib2**: é o seu arquivo grib2

**vel850hpa.grib2**: é o arquivo que será gerado com a velocidade do vento. Altere o nome de acordo com a sua necessidade.

Caso seja necessário gerar a velocidade e a direção do vento no mesmo arquivo, basta fazer:

``` wgrib2 -match ':(UGRD|VGRD):850 mb' gfs_f001_20230414.grib2 -wind_dir dir_vel850hpa.grib2 -wind_speed dir_vel850hpa.grib2```

Nota-se que, o **arquivo a ser gerado é o mesmo** na função ```-wind_dir``` e ```-wind_speed```, isto é, ```dir_vel850hpa.grib2```. Isso ocorre para que os resultados sejam armazenados no mesmo arquivo.

##### Selecionar os níveis verticais de um arquivo

A partir de um arquivo de uma simulação do modelo GFS, deseja-se visualizar as variáveis que estão nele:

```wgrib2 gfs.0p25.2018042200.f006.grib2```

Supondo que deseja-se trabalhar com a variável ```TMP```. O objetivo consiste em salvar apenas esta variável no formato NetCDF. Para isso, basta digitar o comando abaixo:

```wgrib2 -match 'TMP' gfs.0p25.2018042200.f006.grib2 -netcdf out.TMP.nc```

O problema é que são geradas muitas ocorrências que contém a variável ```TMP```. 

Neste caso, imagina-se que se deseja apena a ```TMP a 2 metros```. O comando a ser utilizado será:

```wgrib2 -match ':TMP:2 m above ground:' gfs.0p25.2018042200.f006.grib2 -netcdf out.TMP.nc```

O arquivo gerado contém apenas a variável ```TMP a 2 metros```. Como sabemos disso? Basta digitar o comando abaixo:

```ncdump -h out.TMP.nc```

Há um trecho do comando acima que mostrará a seguinte informação:

```float TMP_2maboveground(time, latitude, longitude)```

Aqui, foi tranquilo. Agora, vamos dificultar um pouco. Supondo que se deseja obter a ```TMP``` somente para todos os níveis verticais. E agora? Basta usar o comando abaixo:

```wgrib2 gfs.0p25.2018042200.f006.grib2 -match "(:TMP:[0-9]* mb:)" -netcdf out.TMP.nc```

O que foi gerado? Ao digitar o comando abaixo:

```ncdump -h out.TMP.nc```

Nota-se que, a variável ```TMP``` veio com os níveis verticais separados. Como se fossem variáveis independentes, e não é isso que se deseja. O objetivo, é ter a variável ```TMP``` com todos os níveis verticais juntos, e não separados. Para isso, utiliza-se o mesmo comando com o parâmetro ```-nc_nlev```.

```wgrib2 gfs.0p25.2018042200.f006.grib2 -nc_nlev 31 -match "(:TMP:[0-9]* mb:)" -netcdf out.TMP.nc```

O parâmetro ```-nc_nlev 31``` diz que são 31 níveis verticais que estão no arquivo.

Agora, deseja-se selecionar alguns níveis verticais em particular. Basta fazer:

```wgrib2 gfs.0p25.2018042200.f006.grib2 -nc_nlev 5 -match ":TMP:(600|650|700|750|800) mb:" -netcdf out.TMP.nc```

Lembrando que foram selecionados apenas 5 níveis verticais, por isso, ```-nc_nlev 5```.

Digite o comando:

```ncdump -c out.TMP.nc```

Na última linha, terá a seguinte informação:

```plevel = 600, 650, 700, 750, 800 ;```

São os níveis verticais selecionados.

##### Selecionar mais de uma variável e vários níveis verticais de um arquivo

Agora, deseja-se selecionar alguns níveis verticais em particular. Basta fazer:

```wgrib2 gfs.0p25.2018042200.f006.grib2 -nc_nlev 5 -match ":(TMP|RH):(600|650|700|750|800) mb:" -netcdf out.TMP.nc```

Onde:

* TMP: é a temperatura
* RH: é a umidade relativa

Para adicionar mais variáveis, basta fazer:

```wgrib2 gfs.0p25.2018042200.f006.grib2 -nc_nlev 5 -match ":(TMP|RH|HGT):(600|650|700|750|800) mb:" -netcdf out.TMP.nc```

Neste novo exemplo, adicionou-se a variável ```HGT```.