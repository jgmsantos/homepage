Processamento de arquivo texto
==============================

Os comandos são aplicados em arquivos no formato texto. As dicas foram obtidas da internet e de anotações pessoais. O símbolo `>` direciona a saída do comando para um arquivo texto. Os símbolos `^` e `$` significam início e fim de uma linha, respectivamente.

### Dicas utilizando o sed

+ Deleta as linhas em branco de um arquivo: 

`sed '/^$/d' seu.arquivo.texto > out.txt`

+ Deleta a partir da linha 3 de duas em duas linhas: 

`sed '3~2d' seu.arquivo.texto > out.txt`

+ Imprime as linhas pares: 

`sed '1~2d' seu.arquivo.texto > out.txt`

+ Imprime as linhas ímpares: 

`sed '2~2d' seu.arquivo.texto > out.txt`

+ Imprime uma linha específica. Exemplo: Imprime somente a linha 27: 

`sed '27!d' seu.arquivo.texto > out.txt`

+ Imprime  um intervalo de linhas. Exemplo: Imprime da linha 10 até 15: 

`sed '10,15!d' seu.arquivo.texto > out.txt`

+ Realiza substituição de texto no arquivo (opção "g" substitui em todo o arquivo): 

`sed 's/zonal wind/vento zonal/g' seu.arquivo.texto > out.txt`

+ Realiza substituição no próprio arquivo (opção "-i"): 

`sed -i 's/\./,/g' seu.arquivo.texto`

+ Substitui TAB por nada no início da linha:

`sed 's/^[ \t]*//' seu.arquivo.texto > out.txt`

+ Substitui a palavra "texto" por nada: 

`sed 's/texto//g' seu.arquivo.texto > out.txt`

+ Substitui ponto (.) por vírgula (,): 

`sed 's/\./,/g' seu.arquivo.texto > out.txt`

+ Transforma coluna em linha: 

`sed 's/  /\n/g' seu.arquivo.texto > out.txt`

### Dicas utilizando o AWK

+ Imprime as linhas pares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 != {print}' > out.txt`

+ Imprime as linhas ímpares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 == {print}' > out.txt`

+ Transforma coluna em linha: 

`cat seu.arquivo.texto | awk '{gsub(/  /,"\n");print}' > out.txt`

+ Deleta a coluna 1 ($1) de um arquivo texto: 

`cat seu.arquivo.texto | awk '{$1=""; print}' > out.txt`

### Dicas usando o xargs

+ Transforma linha em coluna: 

`cat seu.arquivo.texto | xargs`

### Dicas usando o find

+ Excluir arquivos vazios no diretório corrente (.): 

`find . -type f -empty | xargs rm`

+ Excluir diretório vazios no diretório corrente (.): 

`find . -type d -empty | xargs rm -rf`

### Dicas usando Shell

#### Links interessantes

+ [Sams Teach Yourself Shell Programming in 24 Hours](https://drive.google.com/open?id=1b6wHLgNByMUT06tkx-o2T1EbPJs4zMpV)

+ [Advanced Bash-Scripting Guide. Autor: Mendel Cooper.](https://drive.google.com/open?id=16GPHZQK2Bsd_FmfPx_MirrMcEwwMuZYc)

+ [Linux Shell Scripting Tutorial v1.05r3 A Beginner's handbook. Autor: Vivek G. Gite.](https://drive.google.com/open?id=1HIveI42BcDEN4SE7XgzS6easdEmgN83n)

+ [Manipulating Strings](https://drive.google.com/file/d/1-_c7zeNH_aI2KQhZ4ZufdvBd41jzwiqG/view?usp=sharing)

+ [String Operations in Shell](https://drive.google.com/file/d/152p14yGH4NpB6JCqqD5unGSl8H9J2lv_/view?usp=sharing)

+ [Softpanorama](http://www.softpanorama.org/Scripting/Shellorama/String_operations/index.shtml)
	
+ [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/string-manipulation.html)

#### Exemplos de scrips em Shell

1 Uso do loop `while read`

+ O arquivo abaixo (`01lat_lon.txt`) possui 3 colunas (longitude, latitude e um código do local) e 15 linhas. O objetivo consiste em realizar um loop em cada linha do arquivo texto para demonstrar o uso do `while read`.
+ [Clique aqui](https://github.com/jgmsantos/Scripts/tree/master/SHELL) para realizar o download do script `01loop.sh`.
+ [Clique aqui](https://github.com/jgmsantos/Scripts/tree/master/SHELL) para realizar o download do arquivo texto `01lat_lon.txt`.
+ Tornando o script executável: `chmod +x 01loop.sh`.
+ Executando o script: `./01loop.sh`.

```
-72	-33	1
-71	-33	2
-70	-33	3
-69	-33	4
-68	-33	5
-67	-33	6
-66	-33	7
-65	-33	8
-64	-33	9
-63	-33	10
-62	-33	11
-61	-33	12
-60	-33	13
-59	-33	14
-58	-33	15
```

+ O script `01loop.sh` é demonstrado abaixo:
```
#/bin/bash

while read linha ; do
    lon=$(echo ${linha} | awk {'print $1'})
    lat=$(echo ${linha} | awk {'print $2'})
    id=$(echo ${linha} | awk {'print $3'} | awk '{printf("%.3d",$1)}')

    echo ${lat} ${lon} ${id}

    # Suas instruções...
done < 01lat_lon.txt
```

2 Baixar dados de umidade do solo do produto GRACE da NASA utilizando o wget.

+ O GRACE é um produto de estimativa de umidade do solo baseado em observações de armazenamento de água terrestre derivadas do satélite Gravity Recovery and Climate Experiment Follow On (GRACE-FO) e integradas a outras observações, usando um modelo numérico sofisticado de processos de água e energia da superfície terrestre.
+ Descreve as condições atuais de umidade ou seca expressa como um percentil - probabilidade de ocorrência de seca ou não para um determinado local e época do ano.
+ São disponibilizadas 3 variávei: Groundwater Percentile (gws), 
Root Zone Soil Moisture Percentile (rtzsm) e Surface Soil Moisture Percentile (sfsm).
Disponibilidade: a cada 7 dias (3 de fevereiro de 2003 - atual).
+ Atualização: mais ou menos 3 dias.
+ Resolução: global: 25 km x 25 km e EUA: 12,5 km x 12,5 km.
+ Link: https://nasagrace.unl.edu/globaldata

+ [Clique aqui](https://github.com/jgmsantos/Scripts/tree/master/SHELL) para realizar o download do script `02get_data_NASA_GRACE.sh`. NÃO DEIXE DE LER AS INSTRUÇÕES PARA EXECUTAR ADEQUADAMENTE O SCRIPT.

