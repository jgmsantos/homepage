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

6 O objetivo do script abaixo consiste em converter vários  arquivos no formato texto com 6 colunas com resolução temporal a cada 3 hora, isto é, uma série temporal para o formato NetCDF. O arquivo não apresenta nível vertical.

+ O exemplo foi obtido do link abaixo e posteriormente adaptado:
  + [https://code.mpimet.mpg.de/boards/2/topics/8771](https://code.mpimet.mpg.de/boards/2/topics/8771)
  + Para o download dos arquivos [clique aqui](../files/cdo01.zip)

+ O script abaixo realizará essa tarefa e possui o nome de `txt2nc.sh`. O seu conteúdo é exibido logo a seguir.

+ Para torná-lo executável, basta digitar:

  `chmod +x txt2nc.sh`

+ Para executá-lo, basta digitar:

  `./txt2nc.sh`

+ Não se preocupe com as mensagens de Warning (Aviso) que vão aparecer.

```
#!/bin/bash

rm -f estacao_???.nc # Remove os arquivos para não gerar error.

ncols=6 # Número de variáveis.

# Cada coluna do arquivo texto é representada 
# pelo nome da variável/unidade na lista abaixo.
#               Col1  Col2  Col3   Col4 Col5  Col6
nome_variavel=("prec" "vel" "temp" "HR" "SWR" "LWR")
unidade_variavel=("mm" "m/s" "C" "HR" "W/m2" "W/m2")

# Altere a data conforme a resolução temporal do seu 
# arquivo. A unidade pode ser: hour, day, mon e year.
# O arquivo a ser gerado terá resolução temporal a cada
# 3 horas (3hour) começando em 01/01/1950.
time="1950-01-01,00:00:00,3hour"

k=1

for var in $(ls data_*) ; do 

   id=$(echo ${k} | awk '{printf("%.3d",$1)}')  # Nome do arquivo de saída.
   lat=$(echo ${var} | cut -d "_" -f 2)  # Latitude do local.
   lon=$(echo ${var} | cut -d "_" -f 3)  # Longitude do local.
   
   # Descrição do domínio a ser gerado.
   cat << EOF > grid.txt
   gridtype = lonlat
   xsize    = 1
   ysize    = 1
   xvals    = ${lon}
   yvals    = ${lat}
EOF

   j=0
   
   # Loop sobre todas as variáveis.
   for i in $(seq 1 ${ncols}) ; do
       echo "ID: ${id} -> variavel: ${nome_variavel[j]}"
	   
       $(cat  ${var} | cut -d " " -f ${i} > tmp.txt)
       cdo -s -f nc -setattribute,${nome_variavel[$j]}@units=${unidade_variavel[$j]} \ 
                    -settaxis,${time} -setname,${nome_variavel[$j]} \
                    -setmissval,-999 -input,grid.txt out_${i}.nc < tmp.txt
       j=$(expr $j + 1)
   done

   # Junta todos os arquivos em um NetCDF para cada localidade.
   cdo -s -O merge out_*.nc estacao_${id}.nc

   rm -f out_*.nc tmp.txt grid.txt  # Remove arquivos desnecessários.

   k=$(expr $k + 1)
done
```

Ao executar o script, serão mostradas as linhas abaixo:

```
ID: 001 -> variavel: prec
ID: 001 -> variavel: vel
ID: 001 -> variavel: temp
ID: 001 -> variavel: HR
ID: 001 -> variavel: SWR
ID: 001 -> variavel: LWR
Warning: Duplicate entry of parameter -1 in out_2.nc!
Warning: Duplicate entry of parameter -1 in out_3.nc!
Warning: Duplicate entry of parameter -1 in out_4.nc!
Warning: Duplicate entry of parameter -1 in out_6.nc!
ID: 002 -> variavel: prec
ID: 002 -> variavel: vel
ID: 002 -> variavel: temp
ID: 002 -> variavel: HR
ID: 002 -> variavel: SWR
ID: 002 -> variavel: LWR
Warning: Duplicate entry of parameter -1 in out_2.nc!
Warning: Duplicate entry of parameter -1 in out_3.nc!
Warning: Duplicate entry of parameter -1 in out_4.nc!
Warning: Duplicate entry of parameter -1 in out_6.nc! 
```

+ São gerados dois arquivos com os nomes de `estacao_001.nc` e `estacao_002.nc`.

+ Faça as devidas adaptações para os seus arquivos.

### Vídeo aula de CDO

+ [Dia 1 - 25 Outubro de 2018](https://www.youtube.com/watch?v=9IQ9fNlnkUo&t=1232s)

+ [Dia 2 - 26 Outubro de 2018](https://www.youtube.com/watch?v=VSPjY2GaX1M&t=27s)

### Podcast

+ [EPISÓDIO 22 - CDO (CLIMATE DATA OPERATORS - Novembro de 2018)](https://soundcloud.com/cptecinpe/episodio-22)