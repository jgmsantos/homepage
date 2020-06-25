Processamento de arquivo texto
==============================

Os comandos são aplicados em arquivos no formato texto. As dicas foram obtidas da internet e de anotações pessoais. O símbolo `>` direciona a saída do comando para um arquivo texto. Os símbolos `^` e `$` significam início e fim de uma linha, respectivamente.

### Dicas utilizando o sed

1. Deleta as linhas em branco de um arquivo: 

`sed '/^$/d' seu.arquivo.texto > out.txt`

2. Deleta a partir da linha 3 de duas em duas linhas: 

`sed '3~2d' seu.arquivo.texto > out.txt`

3. Imprime as linhas pares: 

`sed '1~2d' seu.arquivo.texto > out.txt`

4. Imprime as linhas ímpares: 

`sed '2~2d' seu.arquivo.texto > out.txt`

5. Imprime uma linha específica. Exemplo: Imprime somente a linha 27: 

`sed '27!d' seu.arquivo.texto > out.txt`

6. Imprime  um intervalo de linhas. Exemplo: Imprime da linha 10 até 15: 

`sed '10,15!d' seu.arquivo.texto > out.txt`

7. Realiza substituição de texto no arquivo (opção "g" substitui em todo o arquivo): 

`sed 's/zonal wind/vento zonal/g' seu.arquivo.texto > out.txt`

8. Realiza substituição no próprio arquivo (opção "-i"): 

`sed -i 's/\./,/g' seu.arquivo.texto`

9. Substitui TAB por nada no início da linha:

`sed 's/^[ \t]*//' seu.arquivo.texto > out.txt`

10. Substitui a palavra "texto" por nada: 

`sed 's/texto//g' seu.arquivo.texto > out.txt`

11. Substitui ponto (.) por vírgula (,): 

`sed 's/\./,/g' seu.arquivo.texto > out.txt`

12. Transforma coluna em linha: 

`sed 's/  /\n/g' seu.arquivo.texto > out.txt`

### Dicas utilizando o AWK

1. Imprime as linhas pares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 != {print}' > out.txt`

2. Imprime as linhas ímpares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 == {print}' > out.txt`

3. Transforma coluna em linha: 

`cat seu.arquivo.texto | awk '{gsub(/  /,"\n");print}' > out.txt`

4. Deleta a coluna 1 ($1) de um arquivo texto: 

`cat seu.arquivo.texto | awk '{$1=""; print}' > out.txt`

### Dicas usando o xargs

1. Transforma linha em coluna: 

`cat seu.arquivo.texto | xargs`

### Dicas usando o find

1. Excluir arquivos vazios no diretório corrente (.): 

`find . -type f -empty | xargs rm`

2. Excluir diretório vazios no diretório corrente (.): 

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

#### Exemplos de scripts em Shell

1. Uso do loop `while read`

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

2. Baixar dados de umidade do solo do produto GRACE da NASA utilizando o wget.

+ O GRACE é um produto de estimativa de umidade do solo baseado em observações de armazenamento de água terrestre derivadas do satélite Gravity Recovery and Climate Experiment Follow On (GRACE-FO) e integradas a outras observações, usando um modelo numérico sofisticado de processos de água e energia da superfície terrestre.
+ Descreve as condições atuais de umidade ou seca expressa como um percentil - probabilidade de ocorrência de seca ou não para um determinado local e época do ano.
+ São disponibilizadas 3 variáveis: Groundwater Percentile (gws), 
Root Zone Soil Moisture Percentile (rtzsm) e Surface Soil Moisture Percentile (sfsm).
Disponibilidade: a cada 7 dias (3 de fevereiro de 2003 - atual).
+ Atualização: mais ou menos 3 dias.
+ Resolução: global: 25 km x 25 km e EUA: 12,5 km x 12,5 km.
+ Link: [https://nasagrace.unl.edu/globaldata](https://nasagrace.unl.edu/globaldata).

+ Requisitos para executar o script:
    + [CDO](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/cdo.html) e [gdal](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/gdal.html).
+ [Clique aqui](https://github.com/jgmsantos/Scripts/tree/master/SHELL) para realizar o download do script `02get_data_NASA_GRACE.sh`. NÃO DEIXE DE LER AS INSTRUÇÕES PARA EXECUTAR ADEQUADAMENTE O SCRIPT.

3. Script `03limpa_cmip5.sh` feito em Shell conserta a data do modelo HADGEM2-ES. Essse modelo possui dados diários totalizando apenas 360 dias por ano. O objetivo foi corrigir essas datas acrescentando o dia 31 aos meses que possuem apenas essa quantidade de datas, em outras palavras, criou-se apenas o dia 31 com valores UNDEF. Para ano bissexto (366 dias), esse modelo mostra o dia 01 de março duplicado, e para anos normais (365 dias), os dias 01 e 02 são duplicados, e os mesmos são removidos. 

+ NÃO DEIXE DE LER O SCRIPT PARA MAIS INFORMAÇÕES.
+ O script é demonstrado abaixo e o seu download pode ser feito [clicando aqui](https://github.com/jgmsantos/Scripts/blob/master/SHELL/03limpa_cmip5.sh).
+ Requisitos: ncdump e CDO instalados. O ncdump já vem instalado nativamente quando se tem a biblioteca NetCDF. 
+ Baixe o arquivo `03limpa_cmip5.sh`.
+ Sinta-se livre para realizar modificações e ao realizá-las, compartilhe conosco.

```bash
#!/bin/bash

#########################################
# Obtenção do tempo de máquina utilizado. 
# NÃO DELETAR ESSA LINHA!
datainicial=`date +%s` 
#########################################

####################################################################################################
# Esse script conserta as datas das simulações diárias de precipitação do modelo HADGEM2-ES do CMIP5. 
# Esse modelo possui apenas 360 dias de dados para os cenários historical, rcp26 e rcp85, 
# ou seja, os dias 31 não existem no arquivo. Este escript complementa esses dias faltantes, 
# isto é, o dia 31 de cada mês para cada ano abordado.
# Outro detalhe sobre este modelo está no fato de que para anos bissextos, o dia 01 de março 
# é duplicado, e para anos normais, os dias 01 e 02 são duplicados. Essas datas duplicadas são 
# removidas.
# Nome dos arquivos: 
# 1) prec.HADGEM2-ES.historical.nc,
# 2) prec.HADGEM2-ES.rcp26.nc e 
# 3) prec.HADGEM2-ES.rcp85.nc.
# Nome da variável do arquivo: pr. Unidade: mm/dia. Cobertural espacial: Globo.
# Instrução de uso: chmod +x limpa.sh e para executar: ./limpa.sh
# Crie um diretório e coloque os seus arquivos e este script no mesmo local.
# O arquivo final será gerado no diretório `processado` com o nome: prec.HADGEM2-ES.historical.corrigido.nc, HADGEM2-ES.rcp26.corrigido.nc e HADGEM2-ES.rcp85.corrigido.nc
# Os demais processamento auxiliares serão gerados em `tmp`.
# Autor: Guilherme Martins -> E-mail: jgmsantos@gmail.com -> Site: guilherme.readthedocs.io 
# Data: 23/06/2020 - 22h15 BRT.
# Tempo total de execução: Aproximadamente 00:05:00 para cada cenário.
####################################################################################################

modelo="HADGEM2-ES"  # Nome do modelo
nome_variavel="pr"  # Nome da variável do arquivo.
DIR_OUTPUT="./processado"
DIR_TMP="./tmp"

# Os diretórios abaixo serão criados, caso eles não existam.
if [ ! -e ${DIR_OUTPUT} -a ! -e ${DIR_TMP} ]
then
   mkdir -p ${DIR_INPUT} ${DIR_TMP}
fi

for cenario in "historical" "rcp26" "rcp85" # Cenários a serem processados.
do

    cdo -s griddes prec.${modelo}.${cenario}.nc > ${DIR_TMP}/grid.txt

    if [ ${cenario} = "historical" ]
    then
        anoi="1975"  # Ano inicial.
        anof="1978"  # Ano final.
        timestep1="61"  # Ano bissexto.
        timestep2="60,61"  # Ano normal.
    fi

    if [ ${cenario} = "rcp26" -o ${cenario} = "rcp85" ]
    then
        anoi="2071"  # Ano inicial.
        anof="2074"  # Ano final.
        timestep1="60"  # Ano bissexto.
        timestep2="59,60"  # Ano normal.
    fi

    # Cria um campo constante que será utilizado como o dia 31 de cada mês.
    cdo -s -r -f nc -chname,const,${nome_variavel} -setmissval,-999 -const,-999,${DIR_TMP}/grid.txt ${DIR_TMP}/campo_constante.nc

    # Separa o arquivo por anos.
    cdo -s splityear prec.${modelo}.${cenario}.nc ${DIR_TMP}/ano.

    for ano in $(seq ${anoi} ${anof})
    do
        echo ${cenario} ${ano}

        for mes in 01 03 05 07 08 10 12 # Apenas os meses que possuem 31 dias.
        do
            # Separa o arquivo por meses.
            cdo -s -splitmon ${DIR_TMP}/ano.${ano}.nc ${DIR_TMP}/tmp02.${ano}.mes.

            # Obtém a data final de cada arquivo que contém o mês de 31 dias. 
            data_inicial=$(cdo -s infon ${DIR_TMP}/tmp02.${ano}.mes.${mes}.nc | tail -1 | sed 's/-//g' | awk '{print $3}')
            # Obtém a data final de cada mês de 31 dias. É feita é uma soma de um dia para obter a data real
            # Exemplo: O mês de janeiro será mostrado como dia 30, e a linha abaixo soma mais um dia, para ficar 
            # com o dia correto, isto é, dia 31.
            data_final=$(date '+%Y%m%d' -d "${data_inicial} +1 days")
            # Essa sepação foi feita para consertar a data utilizando o CDO (settaxis).
            ano=${data_final:0:4}
            mes=${data_final:4:2}
            dia=${data_final:6:2}
            # Hora final, essa hora será utilizada no CDO (settaxis).
            hora_inicial=$(cdo -s infon ${DIR_TMP}/tmp02.${ano}.mes.${mes}.nc | tail -1 | awk '{print $4}')
            # Obtém o valor UNDEF para manter o mesmo padrão de valores do arquivo original.
            fillvalue=$(ncdump -h ${DIR_TMP}/tmp02.${ano}.mes.${mes}.nc | grep ${nome_variavel}:_FillValue | awk '{print $3}' | sed 's/f//')

            echo $data_inicial $data_final $hora_inicial

            # Obtido o campo constante, conserta-se sua data, apenas isso.
            cdo -s -setmissval,$fillvalue -settaxis,${ano}-${mes}-${dia},${hora_inicial},1day ${DIR_TMP}/campo_constante.nc ${DIR_TMP}/tmp03.${ano}.mes.${mes}.nc

            # O mergetime tem como objetivo unir os meses com 30 dias com o campo constante (dia 31), gerando assim, um arquivo
            # com 31 dias.
            cdo -s -O mergetime ${DIR_TMP}/tmp02.${ano}.mes.${mes}.nc ${DIR_TMP}/tmp03.${ano}.mes.${mes}.nc ${DIR_TMP}/tmp04.${ano}.mes.${mes}.nc

        done

        # Dessa vez, os meses para um ano em particular são unidos de forma a terem 365 ou 366 dias.
        cdo -s -O mergetime ${DIR_TMP}/tmp04.${ano}.mes.01.nc ${DIR_TMP}/tmp02.${ano}.mes.02.nc \
                            ${DIR_TMP}/tmp04.${ano}.mes.03.nc ${DIR_TMP}/tmp02.${ano}.mes.04.nc  \
                            ${DIR_TMP}/tmp04.${ano}.mes.05.nc ${DIR_TMP}/tmp02.${ano}.mes.06.nc  \
                            ${DIR_TMP}/tmp04.${ano}.mes.07.nc ${DIR_TMP}/tmp04.${ano}.mes.08.nc  \
                            ${DIR_TMP}/tmp02.${ano}.mes.09.nc ${DIR_TMP}/tmp04.${ano}.mes.10.nc  \
                            ${DIR_TMP}/tmp02.${ano}.mes.11.nc ${DIR_TMP}/tmp04.${ano}.mes.12.nc  \
                            ${DIR_TMP}/tmp05.${ano}.nc

        if [ $(expr ${ano} % 4) -eq 0 ]  # Checa se o ano é bissexto ou não.
        then
           # Após ter obtido o arquivo com 366 dias, a linha abaixo, remove a data duplicada, isto é, o dia 01 de março.
           # Isto é feito para cada ano.
           cdo -s -delete,timestep=${timestep1} ${DIR_TMP}/tmp05.${ano}.nc ${DIR_TMP}/tmp06.${ano}.nc  # O mês de março tem o dia 01 duplicado.
        else
           # Após ter obtido o arquivo com 365 dias, a linha abaixo, remove as datas duplicadas, isto é, os dias 01 e 02 de março.
           # Isto é feito para cada ano.
           cdo -s -delete,timestep=${timestep2} ${DIR_TMP}/tmp05.${ano}.nc ${DIR_TMP}/tmp06.${ano}.nc  # O mês de março tem os dias 01 e 02 duplicados.
        fi
    done

    # Por fim, todos os anos são unidos em um único arquivo.
    cdo -s -O mergetime ${DIR_TMP}/tmp06.????.nc ${DIR_OUTPUT}/prec.HADGEM2-ES.${cenario}.corrigido.nc

    # Remove arquivos desnecessários.
    rm -f ${DIR_TMP}/ano.????.nc ${DIR_TMP}/tmp??.*.nc ${DIR_TMP}/campo_constante.nc ${DIR_TMP}/grid.txt

done

#####################################
datafinal=`date +%s`
soma=`expr $datafinal - $datainicial`
resultado=`expr 10800 + $soma`
tempo=`date -d @$resultado +%H:%M:%S`
echo " Tempo gasto: $tempo "
#####################################

```

+ Ao executar o script`03limpa_cmip5.sh`, parte do resultado para o cenário historical é mostrado na tela do seu computador com a seguinte mensagem:

```
historical 1975
19750130 19750131 12:00:00
19750330 19750331 12:00:00
19750530 19750531 12:00:00
19750730 19750731 12:00:00
19750830 19750831 12:00:00
19751030 19751031 12:00:00
19751230 19751231 12:00:00
```

Os arquivos de saída terão os seguintes nomes: 
+ `prec.HADGEM2-ES.historical.corrigido.nc`
+ `prec.HADGEM2-ES.rcp26.corrigido.nc`
+ `prec.HADGEM2-ES.rcp85.corrigido.nc`