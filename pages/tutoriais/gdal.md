gdal
====

### Link para o site do gdal 

+ [http://www.gdal.org](http://www.gdal.org)
+ [http://www.gdal.org/gdal_translate.html](http://www.gdal.org/gdal_translate.html)
+ [http://www.gdal.org/formats_list.html](http://www.gdal.org/formats_list.html)


### Instalando o gdal no Linux

`sudo apt install gdal-bin`

### Exemplos de uso do gdal.

1 Convertendo um arquivo NetCDF para o formato binário.

+ Criar o arquivo descritor (ctl) com o cdo:

`cdo gradsdes input.nc`

Será criado o arquivo `input.ctl`. O `input.nc` é o seu arquivo NetCDF.

+ Gerando o binário a partir do NetCDF. Não esquecer de editar o `.ctl` para abrir corretamente o seu binário.

`gdal_translate -of ENVI input.nc output.bin`

2 Convertendo um arquivo NetCDF para o formato tif.

`gdal_translate -of GTiff -a_srs EPSG:4326 input.nc output.tif`

3 Convertendo um arquivo no formato tif para NetCDF.

`gdal_translate -of netcdf -co "FORMAT=NC" input.tif output.nc`

4 Convertendo um arquivo no formato NetCDF para o formato tif e compacta o arquivo (no sentido de reduzir o tamanho ocupado em disco pelo tif).

`gdal_translate -of GTiff -a_srs EPSG:4326 -co TILED=YES -co COPY_SRC_OVERVIEWS=YES -co COMPRESS=LZW input.nc output.tif`

5 Convertendo um arquivo shapefile para NetCDF.

`gdal_rasterize -burn 1 -of netCDF -a_nodata -999 -a_srs epsg:4326 -tr 0.01 0.01 input.shp output.nc`

Onde: `-of` = formato de interesse, `-a_nodata` = valor UNDEF de interesse, `-a_srs epsg` = tipo de projeção e `-tr` = resolução de interesse, nesse caso, 1km.

6 Juntando arquivos tif.

O objetivo consiste em unir (`gdal_merge.py`) dois arquivos, o `VENTO.U10M.GFS.ANL.2020070118.tif` e o `VENTO.V10M.GFS.ANL.2020070118.tif`. 

A ordem de disposição dos arquivos é importante. Ao gerar o `VENTO_UV.tif`, a variável `Band 1` corresponde a componente `u` e `Band 2` a componente `v`.

`gdal_merge.py -separate -o VENTO_UV.tif VENTO.U10M.GFS.ANL.2020070118.tif
VENTO.V10M.GFS.ANL.2020070118.tif`

Onde: `VENTO_UV.tif` é o arquivo com as duas variáveis. Esse nome é definido pelo usuário.

Basta digitar o comando abaixo para ver o conteúdo do arquivo `VENTO_UV.tif`.

`gdalinfo VENTO_UV.tif`