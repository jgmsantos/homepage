gdal
====

### Link para o site do gdal 

+ [http://www.gdal.org](http://www.gdal.org)
+ [http://www.gdal.org/gdal_translate.html](http://www.gdal.org/gdal_translate.html)
+ [http://www.gdal.org/formats_list.html](http://www.gdal.org/formats_list.html)


### Instalando o gdal no Linux

`sudo apt install gdal-bin`

### Exemplos de uso do gdal

#### Convertendo um arquivo NetCDF para o formato binário

+ Criar o arquivo descritor (ctl) com o cdo:

`cdo gradsdes input.nc`

Será criado o arquivo `input.ctl`. O `input.nc` é o seu arquivo NetCDF.

+ Gerando o binário a partir do NetCDF. Não esquecer de editar o `.ctl` para abrir corretamente o seu binário.

`gdal_translate -of ENVI input.nc output.bin`

#### Convertendo um arquivo NetCDF para o formato tif

`gdal_translate -of GTiff -a_srs EPSG:4326 input.nc output.tif`

#### Convertendo um arquivo no formato tif para NetCDF

`gdal_translate -of netcdf -co "FORMAT=NC" input.tif output.nc`

#### Convertendo um arquivo no formato NetCDF para o formato tif e compacta o arquivo (no sentido de reduzir o tamanho ocupado em disco pelo tif)

`gdal_translate -of GTiff -a_srs EPSG:4326 -co TILED=YES -co COPY_SRC_OVERVIEWS=YES -co COMPRESS=LZW input.nc output.tif`

#### Convertendo um arquivo shapefile para NetCDF

`gdal_rasterize -burn 1 -of netCDF -a_nodata -999 -a_srs epsg:4326 -tr 0.01 0.01 input.shp output.nc`

Onde: `-of` = formato de interesse, `-a_nodata` = valor UNDEF de interesse, `-a_srs epsg` = tipo de projeção e `-tr` = resolução de interesse, nesse caso, 1km.

#### Juntando arquivos tif

O objetivo consiste em unir (`gdal_merge.py`) dois arquivos, o `VENTO.U10M.GFS.ANL.2020070118.tif` e o `VENTO.V10M.GFS.ANL.2020070118.tif`. 

A ordem de disposição dos arquivos é importante. Ao gerar o `VENTO_UV.tif`, a variável `Band 1` corresponde a componente `u` e `Band 2` a componente `v`.

`gdal_merge.py -separate -o VENTO_UV.tif VENTO.U10M.GFS.ANL.2020070118.tif
VENTO.V10M.GFS.ANL.2020070118.tif`

Onde: `VENTO_UV.tif` é o arquivo com as duas variáveis. Esse nome é definido pelo usuário.

Basta digitar o comando abaixo para ver o conteúdo do arquivo `VENTO_UV.tif`.

`gdalinfo VENTO_UV.tif`

#### Reclassificar as classes do  MapBiomas

O objetivo consiste em reclassificar as 38 classes da coleção 8 do MapBiomas para 6 classes, isto é:

* Classe 1: Floresta
* Classe 2: Formação Natural não Florestal
* Classe 3: Agropecuária
* Classe 4: Área não Vegetada
* Classe 5: Corpo D'água
* Classe 6: Não observado

Os links abaixo mostram todas as classes da coleção 8 do MapBiomas:

* [Códigos de legenda](https://brasil.mapbiomas.org/codigos-de-legenda/)

* [As 38 classes](https://brasil.mapbiomas.org/wp-content/uploads/sites/4/2023/08/Legenda-Colecao-8-LEGEND-CODE.pdf)

* [Descrição detalhada das 38 classes](https://brasil.mapbiomas.org/wp-content/uploads/sites/4/2023/09/Legenda-Colecao-8-Descricao-Detalhada-PDF-PT-3-1.pdf)

Para recortar o mapa de uso e cobertura da terra do MapBiomas, basta seguir a dica do link abaixo. Lembrando que a resolução original é de 30 metros.

[Recortar um arquivo GeoTIFF utilizando shapefile](https://www.youtube.com/watch?v=tiCxRcr4q3Q&t=4s&ab_channel=CursosLibertatem)

Para fazer a reclassificação, utiliza-se o `gdal_calc.py`.

Script em Shell para realizar a reclassificação:

Para executar o script, basta abrir o seu terminal e digitar o comando abaixo:

```bash
bash classifica_mapbiomas_6classes.sh
```

```bash
#!/bin/bash

# Arquivo que está no seu computador.
Arquivo_Input=sao_paulo_2022.tif
# Arquivo a ser gerado no seu computador.
Arquivo_Output=sao_paulo_2022_6classes.tif

gdal_calc.py -A ${Arquivo_Input} --outfile ${Arquivo_Output} --NoDataValue=0 --calc="\
  1*(A==1)+1*(A==3)+1*(A==4)+1*(A==5)+1*(A==6)+1*(A==49)+\
  2*(A==10)+2*(A==11)+2*(A==12)+2*(A==32)+2*(A==29)+2*(A==50)+2*(A==13)+\
  3*(A==14)+3*(A==15)+3*(A==18)+3*(A==19)+3*(A==39)+3*(A==20)+3*(A==40)+\
  3*(A==62)+3*(A==41)+3*(A==36)+3*(A==46)+3*(A==47)+3*(A==35)+3*(A==48)+\
  3*(A==9)+3*(A==21)+\
  4*(A==22)+4*(A==23)+4*(A==24)+4*(A==30)+4*(A==25)+\
  5*(A==26)+5*(A==33)+5*(A==31)+\
  6*(A==27)"
```

Explicação do comando:

* -A: é o arquivo de entrada.
* --outfile: é o arquivo a ser gerado.
* --NoDataValue: define um valor para dado ausente (undef). Neste caso, o valor zero será undef.
* --calc: responsável pela reclassificação.
  * os números de `1*` a `6*` são as novas classes que serão geradas.
  * 1*(A==1)+1*(A==3)+1*(A==4)+1*(A==5)+1*(A==6)+1*(A==49) significa que do arquivo `A` (sao_paulo_2022.tif), os pixeis com as classes 1, 3, 4, 5, 6, e 49 serão substituídos pelo valor 1.
  * 2*(A==10)+2*(A==11)+2*(A==12)+2*(A==32)+2*(A==29)+2*(A==50)+2*(A==13). Neste caso, as classes 10, 11, 12, 32, 29, 50, 13 terão valor 2.
  * Para as demais classes o raciocínio é o mesmo.

As figuras abaixo mostram o antes e o depois da reclassificação.

* Antes com as 38 classes:
![](../../images/gdal/mapbiomas/antes.JPG)

* Depois com as 6 classes:
![](../../images/gdal/mapbiomas/depois.JPG)

### Download de dados do modelo GFS

O objetivo consiste em selecionar um horário de simulação do modelo GFS e sem realizar o download dele, selecionar algumas variáveis de interesse e salvar esta seleção localmente.

Selecionar variáveis de interesse:

Basta visualizar um dos arquivos com a extensão ```.idx```. Exemplo: ```gfs.t00z.pgrb2.0p50.f012.idx```.

Uma vez selecionada as variáveis, nota-se que tem um número para cada linha do arquivo ```.idx```, este é o número que será utilizado para selecionar as variáveis.

Exemplo: Serão selecionadas as variáveis abaixo: TMP, UGRD, VGRD e APCP que possuem a seguinte númeração: 581, 588, 589 e 596, respectivamente. Lembrando que essa informação veio do arquivo ```.idx```

```bash
581:127174079:d=2024032800:TMP:2 m above ground:12 hour fcst:
588:128579005:d=2024032800:UGRD:10 m above ground:12 hour fcst:
589:128863326:d=2024032800:VGRD:10 m above ground:12 hour fcst:
596:431949438:d=2024032800:APCP:surface:0-3 hour acc fcst:
```

A linha de comando abaixo selecionará essas variáveis e o resultado será guardado no arquivo ```gfs.t00z.pgrb2.0p25.f002.nc```.

```bash
gdal_translate /vsicurl/https://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.20240328/00/atmos/gfs.t00z.pgrb2.0p50.f012 -b 581 -b 588 -b 589 -b 596 -projwin -58 2 -46 -9 -of netcdf -co "FORMAT=NC" gfs.t00z.pgrb2.0p25.f002.nc
```

Explicando o comando:
* Faz o download do arquivo de interesse: 
  * /vsicurl/https://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.20240328/00/atmos/gfs.t00z.pgrb2.0p50.f012 
* Seleciona as bandas (variáveis) de interesse a partir do arquivo ```.idx```: 
  * -b 581 -b 588 -b 589 -b 596
* Recorta o dado na área de interesse. Convenção: longitude oeste (-58), latitude norte (2), longitude leste (-46) e latitude sul (-9):
  * -projwin -58 2 -46 -9
* Salva o arquivo no formato NetCDF:
  * -of netcdf -co "FORMAT=NC"
* Nome do arquivo a ser gerado no computador. É nome definido pelo usuário:
  * gfs.t00z.pgrb2.0p25.f002.nc

Para salvar no formato GeoTIFF:

```bash
gdal_translate /vsicurl/https://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.20240328/00/atmos/gfs.t00z.pgrb2.0p50.f012 -b 581 -b 588 -b 589 -b 596 -projwin -58 2 -46 -9 gfs.t00z.pgrb2.0p25.f002.tif
```

Para salvar todo o domínio, sem recortar o dado, basta remover o parâmetro ```-projwin -58 2 -46 -9```.

```bash
gdal_translate /vsicurl/https://www.ftp.ncep.noaa.gov/data/nccf/com/gfs/prod/gfs.20240328/00/atmos/gfs.t00z.pgrb2.0p50.f012 -b 581 -b 588 -b 589 -b 596 -of netcdf -co "FORMAT=NC" gfs.t00z.pgrb2.0p25.f002.nc
```

Para visualizar o conteúdo do arquivo (ver o nome das bandas ou variáveis), basta fazer:

```bash
gdalinfo gfs.t00z.pgrb2.0p25.f002.nc
```

ou

```bash
gdalinfo gfs.t00z.pgrb2.0p25.f002.tif
```

#### Calcular a velocidade do vento

```bash
gdal_calc.py -U gfs.t00z.pgrb2.0p25.f002.tif --U_band=2 -V gfs.t00z.pgrb2.0p25.f002.tif --V_band=3 --calc="sqrt(U*U+V*V)" --NoDataValue=-999 --format=netcdf --overwrite --outfile velocidade.nc
```

