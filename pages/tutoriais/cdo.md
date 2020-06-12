Climate Data Operators (CDO)
=================================

### Tutorias

+ [Introdução ao Climate Data Operators (CDO). Autor: Guilherme Martins. Versão oficial](http://mtc-m21b.sid.inpe.br/col/sid.inpe.br/mtc-m21b/2016/11.18.17.34/doc/publicacao.pdf)
  + [Download alternativo para o tutorial acima. Versão sendo atualizada constantemente](https://drive.google.com/open?id=15UT8bdlLlwxwazTvRk2IK28oys89Z4nl)

+ [CDO Reference Card](https://code.zmaw.de/projects/cdo/embedded/cdo_refcard.pdf)

+ [CDO User's Guide](https://code.mpimet.mpg.de/projects/cdo/embedded/cdo.pdf)

+ [Analyse and visualize](https://drive.google.com/file/d/1-CjKE9Akr5AZv3wClCSlRduRNgSiqUNV/view?usp=sharing)

+ [Climate Data Operators as a user-friendly tools for CMSAF'S satellite-derived climate monitoring products](https://drive.google.com/file/d/1upkxIQ9Is_66MGr_wfy_1yQIUeipM7U3/view?usp=sharing)

+ [Climate indices with CDO](https://drive.google.com/file/d/1VbCWik2sypCPCBD-kezXR4IsMeUx4m8b/view?usp=sharing)

+ [CDO_Seminar_20161206](https://drive.google.com/file/d/1TcJM0AEI1T0LYqk5341XCwevyg94f39Y/view?usp=sharing)

+ [Resolução de problemas](https://code.mpimet.mpg.de/projects/cdo/wiki/FAQ)

+ [CDO's python bindings](https://drive.google.com/file/d/1LJiBJ0MLVKzlnk4kmSx_6-pL8UnfI3jd/view?usp=sharing)

+ [Data Analysis and Visualizing using CDO and Ferret](https://drive.google.com/file/d/1yK4xGW5ywk04SlP6qeRJnroMKIoUW788/view?usp=sharing)

### Exemplos de uso com o CDO

1 O exemplo abaixo compara dois conjunto de dados espaciais (temperatura [C] e precipitação [mm/dia]) que possuem o mesmo domínio espacial, isto é, o mesmo número de pontos de latitude e longitude no intervalo de 28 dias. Lembrando que esse arquivo é apenas um exemplo, ele poderia ter qualquer comprimento temporal. Quando a precipitação for menor ou igual (`lec`) a 10 mm/dia, o conjunto de dados será convertido para valores 1 (condição verdadeira) e 0 (condição falsa). O mesmo ocorre com o operador `gec`, isto é, quando a temperatura for maior igual a 30C, o conjunto de dados receberá o valor 1, caso contrário, receberá o valor 0. Em outras palavras, serão criados dois conjuntos de dados com valores 0 e 1. O operador `mul` multiplicará os dois conjunto de dados, isto é, a matriz de dados de 0 e 1, e por fim, somará todos os 28 dias ou tempos gerando assim um arquivo com o total de dias quando a condição acima for satisfeita simultaneamente.

+ Os arquivos `temp.MT.nc` e `prec.MT.nc` possuem o mesmo domínio espacial e 28 dias (01 a 28 de maio de 2020).
+ Esse exemplo poderia ser aplicado para qualquer comprimento temporal.
+ Esse exemplo foi obtido em: [https://code.mpimet.mpg.de/boards/2/topics/9338](https://code.mpimet.mpg.de/boards/2/topics/9338).

`cdo -s -timsum -mul -gec,30 temp.MT.nc -lec,10 prec.MT.nc output.nc`

O resultado pode ser visualizado abaixo:

![](../../images/cdo_fig01_MT.png)

2 Alterar o nome da variável de vários arquivos em lote no formato NetCDF. É feito um loop utilizando o `for` em todos os arquivos NetCDF (`*.nc`), em seguida, com o uso do CDO por meio do operador `chname` altera-se o nome da variável do arquivo de `nome_antigo` para `nome_novo`. 

`for arquivo in $(ls -1 *.nc); do cdo -s chname,nome_antigo,nome_novo $arquivo $arquivo; done`

3 Verificar o nome da variável dos arquivos NetCDF utilizando o loop `for`.

`for arquivo in $(ls -1 *.nc); do cdo -s pardes $arquivo; done`

4 Substitui espaço por vírgula e substitui a primeira ocorrência de vírgula por “nada”.

+ O comando abaixo do CDO utilizando o operador `showlevel` do CDO mostrará a seguinte informação:

`cdo -s showlevel air.mon.mean.nc`

+ Resultado do comando acima:

`1000 925 850 700 600 500 400 300 250 200 150 100 70 50 30 20 10`

+ O objetivo consiste em substituir os `espaços em branco` por "`,`". Isso é feito com o comando abaixo utilizando o `tr` e o `sed`.

`cdo -s showlevel vwnd.nc | tr ' ' ',' | sed 's/,//'`

5 Utilizando a variável risco de fogo para obter o total de píxeis para cada categoria. O nome da variável do arquivo `tmp.nc` se chama `rbf` e a variável `x` pode ser qualquer nome.
```
cdo -s -output -fldsum -setmissval,0 -expr,'x=rbf<=0.15' tmp.nc
cdo -s -output -fldsum -setmissval,0 -expr,'x=rbf>0.15 && rbf<=0.40' tmp.nc
cdo -s -output -fldsum -setmissval,0 -expr,'x=rbf>0.40 && rbf<=0.70' tmp.nc
cdo -s -output -fldsum -setmissval,0 -expr,'x=rbf>0.70 && rbf<=0.95' tmp.nc
cdo -s -output -fldsum -setmissval,0 -expr,'x=rbf>0.95' tmp.nc
```

### Vídeo aula de CDO

+ [Dia 1 - 25 Outubro de 2018](https://www.youtube.com/watch?v=9IQ9fNlnkUo&t=1232s)

+ [Dia 2 - 26 Outubro de 2018](https://www.youtube.com/watch?v=VSPjY2GaX1M&t=27s)

### Podcast

+ [EPISÓDIO 22 - CDO (CLIMATE DATA OPERATORS - Novembro de 2018)](https://soundcloud.com/cptecinpe/episodio-22)