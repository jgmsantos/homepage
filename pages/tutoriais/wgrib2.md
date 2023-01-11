Instalação do wgrib2
========================

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

Exemplos de uso do wgrib2

1. Visualizar informações sobre o arquivo:

`wgrib2 input.grb2`

2. Converter grib2 para NetCDF:

`wgrib2 gfs.t18z.pgrb2.0p25.anl -netcdf output.nc`

3. Selecionar uma variável `(‘:TMP:’)` de interesse e salva no formato NetCDF:

` wgrib2 -match ':TMP:' gfs.t18z.pgrb2.0p25.anl -netcdf output.nc`

4. Selecionar uma variável de interesse e recortar o dado em um domínio particular:

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