Aplicações variadas
=================================

### Descompactar um arquivo para um diretório específico

`tar -zxvf arquivo.tar.gz -C <nome_diretório>`

### Loop com tempo

1 Loop a cada 30 minutos.

```bash
#!/bin/bash

data_inicial="202001011400" # Formato YYYYMMDDhhmm
data_final="202001011600"   

while [ ${data_inicial} -le ${data_final} ]; do

    echo  ${data_inicial}

    curr=`date -d "${data_inicial:0:8} ${data_inicial:8:2}:${data_inicial:10:2} 30 minutes " +"%Y%m%d %H:%M"`

    # Suas instruções...

    data_inicial=`date -d "${curr}" +%Y%m%d%H%M`
 
    # Recorte nas variáveis para serem utilizadas para outros fins. Esse trecho é opcional.
    ano=${data_inicial:0:4}
    mes=${data_inicial:4:2}
    dia=${data_inicial:6:2}
    hora=${data_inicial:8:2}
    min=${data_inicial:10:2}

done

```

+ O resultado será:

```
202001011400
202001011430
202001011500
202001011530
202001011600
```

2 Outro exemplo com loop, dessa vez, a cada 12 horas.

```bash
data_inicial="2020010100" # AAAAMMDDhh
data_final="2020010300"
 
while [ ${data_inicial} -le ${data_final} ]; do
  
    echo  ${data_inicial}

    # Suas instruções...


    data_inicial=`date -d "${data_inicial:0:8}  ${data_inicial:8:2}:00 12 hours " +"%Y%m%d%H"`   
    
done


```

+ O resultado será:

```
2020010100
2020010112
2020010200
2020010212
2020010300
```

3 Loop variando dia a dia.

```bash
#!/bin/bash

data_inicial="20200101" # Formato AAAAMMDD
data_final="20200110"

while [ ${data_inicial} -le ${data_final} ]
do

    echo ${data_inicial}

    # Suas intruções...

    data_inicial=`date -u +"%Y%m%d" -d "${data_inicial} 1 day "`
done

```

+ O resultado será:

```
20200101
20200102
20200103
20200104
20200105
20200106
20200107
20200108
20200109
20200110
```

4 Outra opção com loop variando dia a dia.

```bash
#!/bin/bash

data_inicial="2020010100" # Formato AAAAMMDD00
data_final="2020011000"

while [ ${data_inicial} -le ${data_final} ]
do

    echo ${data_inicial}

    # Suas intruções...

    data_inicial=$(date -u -d "${data_inicial:0:8} ${data_inicial:8:10}:00 24 hours" +"%Y%m%d%H")
done
```
+ O resultado será:

```
2020010100
2020010200
2020010300
2020010400
2020010500
2020010600
2020010700
2020010800
2020010900
2020011000
```

### Remover caracteres estranhos no Linux

+ Em algumas situações quando se copiam programas feitos no Windows para o Linux, alguns caracteres indesejáveis surgem, gerando o erro abaixo. 

+ Supondo que o script escrito em Shell tenha sido feito no Windows com o nome de `teste.sh`. Ao tentar executá-lo no Linux, a mensagem de erro surgirá:

`./teste.sh: line 2: $'\r': command not found`

+ Esse `\r` é o problema. E isso pode ser resolvido por meio da instalação do `dos2unix` com o comando abaixo:

`sudo apt install dos2unix`

+ Após a instalação, basta digitar:

`dos2unix teste.sh`

+ Para remover os caracteres estranhos. Dessa forma, é possível utilizar o seu script sem problemas.

### Saber o número de núcleos do seu processador

+ Basta digitar no terminal do Linux o comando abaixo:

`nproc`

### Saber qual é a versão do sistema operacioanl Linux

`lsb_release -rd`

+ Resultado:

```
Description:    Ubuntu 18.04.3 LTS
Release:        18.04
```

### Utilizar o wget com usuário e senha

`wget -c -r http://www.siteexemplo.com.br/arquivos.zip --http-user=nome_usuario --http-passwd=senha_usuario`

### Utilizar o wget para alterar o nome do arquivo.

+ Nesse exemplo, será feito o download do arquivo `precip.mon.mean.nc`, e deseja-se que o mesmo tenha outro nome, isto é, `prec.nc`. O comando abaixo realizará essa tarefa.

`wget ftp://ftp.cdc.noaa.gov/Datasets/cmap/enh/precip.mon.mean.nc -O prec.nc`

### Utilizar o rename para renomar arquivos em lote

+ Comando bastante útil para renomear vários arquivos de uma vez ou em lote.

+ Para instalar:

`sudo apt-get install rename`

+ Exemplo de uso. Supondo que se deseja realizar algumas conversões com o `gdal`. Os arquivos de entrada possuem o seguinte nome `YYYYMMDD.tif` (são vários arquivos com datas distintas) e o `for` abaixo alterará o nome desses arquivos para `YYYYMMDD.tif.hard-typed`. A última linha mostrará o uso do comando `rename`. Nota-se que a lógica é semelhante a utilizada no `sed`, isto é, `s` para substituir de uma só vez o `.hard-typed` por `nada` ou `vazio` (`//`) em todos os arquivos.

```bash
#!/bin/bash

for original in $(ls -1 *.tif)
do 
    gdal_translate -of GTiff -ot Float32 -b 1 -co "TILED=YES" ${original} ${original}.hard-typed
done

# Remove arquivos desnecessários.

rm -rf *.tif

# Uso do rename para renomear vários arquivos de uma vez só. O que foi feito? será substituido no nome do arquivo o ".hard-typed" por "espaço vazio" ou //.

rename 's/.hard-typed//' *.hard-typed

```
### Forma alternativa de renomear arquivos em lote usando o loop for e basename

Imagine a seguinte lista de arquivos (6 no total) no formato NetCDF:

```
IGBP_c6_MAPBIOMA_v3_2001_001_RF_cnew.nc
IGBP_c6_MAPBIOMA_v3_2002_001_RF_cnew.nc
IGBP_c6_MAPBIOMA_v3_2003_001_RF_cnew.nc
IGBP_c6_MAPBIOMA_v3_2004_001_RF_cnew.nc
IGBP_c6_MAPBIOMA_v3_2005_001_RF_cnew.nc
IGBP_c6_MAPBIOMA_v3_2006_001_RF_cnew.nc
```
O objetivo consiste em converter do formato [NetCDF para o formato tif](https://guilherme.readthedocs.io/en/latest/pages/tutoriais/gdal.html#exemplos-de-uso-do-gdal) utilizando o `gdal`. Lembrando que isso é apenas um exemplo de como utilizar a lógica do loop `for` e o uso do `basename` para obter apenas o nome do arquivo sem sua extensão.

Esse procedimento é feito com o script abaixo:

```bash
#!/bin/bash

for nome_arquivo in *.nc
do 
    gdal_translate -of GTiff -a_srs EPSG:4326 $nome_arquivo $(basename $nome_arquivo .nc).tif
done
```

O trecho do código `$(basename $nome_arquivo .nc)` imprime apenas o nome do arquivo sem sua extensão, lembrando que esse é o nome do arquivo de saída, resultando em:

`IGBP_c6_MAPBIOMA_v3_2008_001_RF_cnew`

O trecho abaixo:

`$(basename $nome_arquivo .nc).tif`

é o mesmo que:

`IGBP_c6_MAPBIOMA_v3_2006_001_RF_cnew.tif`


### Utilizar o parallel no Linux

+ O `parallel` é excelente quando se deseja realizar várias tarefas de uma só vez. 

+ Para instalar o `parallel` basta digitar:

`sudo apt-get install parallel`

1 Exemplo de como converter arquivos em lote. No exemplo abaixo, há 90 arquivos (01/01/2019 a 31/03/2019) no formato NetCDF com o seguinte nome `RF.ANL.LAT.TOPO.AAAAMMDD00.nc` (AAAA = ano, MM = mês e DD = dia). O objetivo consiste em converter do formato NetCDF para o formato tif utilizando o gdal. Em vez de fazer um loop para converter cada arquivo individualmente, usamos o `parallel` para converter vários arquivos de uma só vez.

+ Mas antes, é imporantante saber o número de processadores do seu computador. Para saber isso, basta digitar no terminal Linux, o comando abaixo:

`nproc`

+ O valor do resultado acima será utilizado na variável `num_processadores` do script abaixo. Por exemplo, o resultado foi 100, claro que não serão utilizados os 100 processadores, mas serão utilizados 30 (escolha aleatória) na variável `num_processadores`.

  + Observação: Nunca utilize todos os processadores do seu computador porque há grande chances de travá-lo, e com isso, a necessidade de reiniciar o sistema.

+ O arquivo `datas.txt` (será criado no script abaixo) conterá a lista com os 90 arquivos (ou dias) de comandos abaixo (mostrando apenas algumas linhas, no total são 90 linhas de comando) que foi gerada dentro do loop do while (do script abaixo). 

+ Apenas lembrando que as linhas abaixo convertem o arquivo NetCDF para tif utilizando o gdal para cada dia:

```
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019010100.nc RF.ANL.LAT.TOPO.2019010100.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019010200.nc RF.ANL.LAT.TOPO.2019010200.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019010300.nc RF.ANL.LAT.TOPO.2019010300.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019010400.nc RF.ANL.LAT.TOPO.2019010400.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019010500.nc RF.ANL.LAT.TOPO.2019010500.tif
.
.
.
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019032700.nc RF.ANL.LAT.TOPO.2019032700.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019032800.nc RF.ANL.LAT.TOPO.2019032800.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019032900.nc RF.ANL.LAT.TOPO.2019032900.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019033000.nc RF.ANL.LAT.TOPO.2019033000.tif
gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019033100.nc RF.ANL.LAT.TOPO.2019033100.tif

```

+ Como foi mencioando, em vez de fazer isso dia a dia por meio de um loop, podemos realizar essa tarefa, por exemplo, executando 30 linhas de comando por vez (`num_processadores` = "30").

+ O comando `split` divide o arquivo `datas.txt` (que contém todas as 90 linhas gerada no loop while) de forma igual, isto é, de 30 em 30 (essa foi a escolha com base no total de processadores do computador), gerando assim os seguintes arquivos abaixo. Após a aplicação do `split` no `datas.txt` que contém 90 linhas de comando, foram gerados os 3 arquivos abaixo:

```
datas_aa
datas_ab
datas_ac
```

+ Cada um desses arquivos (`datas_aa`, `datas_ab` e `datas_ac`) possui 30 linhas de comandos mudando apenas a data.

    + Exemplo de uma linha de comando:
    + gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.2019032700.nc RF.ANL.LAT.TOPO.2019032700.tif      

+ O loop da geração do comando `parallel` será feito com o comando abaixo por meio do script `comandos.sh` (gerado pelo script abaixo):

```
parallel -j 30 -- < datas_aa
parallel -j 30 -- < datas_ab
parallel -j 30 -- < datas_ac
```

+ O script abaixo se chama `executa.sh` (pode ser qualquer nome) que fará todo o serviço:

```bash

#!/bin/bash

data_inicial="20190101"

data_final="20191231"

rm -f datas.txt # Remove o arquivo datas.txt para evitar erro.

# Gera o intervalo de interesse a ser executado.

while [ ${data_inicial} -le ${data_final} ]; do

    echo ${data_inicial}

    # Geração do arquivo datas.txt

    echo "gdal_translate -of GTiff -a_srs EPSG:4326 RF.ANL.LAT.TOPO.${data_inicial}00.nc RF.ANL.LAT.TOPO.${data_inicial}00.tif" >> datas.txt

data_inicial=`date -u +"%Y%m%d" -d "${data_inicial} 1 day "`
done

# Define o número de processadores a serem utilizados com base no comando acima (nproc).

num_processadores="30" 

# Vai gerar novos arquivos a partir de datas.txt que serão separados em 30 (num_processadores) partes iguais.

split -l ${num_processadores} datas.txt datas_

rm -f comandos.sh # Limpa para evitar erro.

# Cria um arquivo chamado comandos.sh que contém todas as linhas de comandos para executar o parallel.

for arq in $(ls -1 datas_*); do
    echo "parallel -j ${num_processadores} -- < $arq" >> comandos.sh
done

chmod +x comandos.sh # Torna o arquivo executável.

nohup ./comandos.sh & # Para executar o script.

exit

```

Depois, basta monitorar o arquivo `nohup` com o comando:

`tail -f nohup`

+ A vantagem de utilizar o `parallel` está no fato de realizar uma tarefa simples executando várias linhas de comando de uma só vez.

2 Esse exemplo consiste em baixar os dados de precipitação diária utilizando o `parallel`. Como afirmado anteriormente, em vez de baixar um arquivo por vez, é possível baixar vários de uma só vez.

+ O script fictício chamado de `get_prec.sh` baixa um arquivo por vez e tem como parâmetro de entrada a data no formato `AAAAMMDD`, com isso é possível realizar o downlod de vários arquivos, claro, respeitando o número de processadores do seu cumputador.

  + Exemplo de como executar: `./get_prec.sh 20200130`

+ Porém, com o uso o `parallel` podemos baixar mais arquivos em lote de acordo conforme o script abaixo:

```bash
#!/bin/bash

data_inicial="20000602"

data_final="20200519"

rm -f datas.txt # Limpa para evitar erro.

# Gera o intervalor de interesse a ser executado.

nome_script="get_prec.sh" # Altere aqui o nome do script.

# Loop no tempo para criar o arquivo datas.txt

while [ ${data_inicial} -le ${data_final} ]; do

    echo ${data_inicial}

    echo "./${nome_script} ${data_inicial}" >> datas.txt

    data_inicial=`date -u +"%Y%m%d" -d "${data_inicial} 1 day "`
done

num_processadores="40" # Número de processadores a serem utilizados.

# Vai gerar novos arquivos a partir de datas.txt que serão separados em 40 (num_processadores) partes iguais.

split -l ${num_processadores} datas.txt datas_

rm -f comandos.sh # Limpa para evitar erro.

# Cria um arquivo chamado comandos.sh que contém todas as linhas de comandos para executar em paralelo.

for arq in $(ls -1 datas_*); do
    echo "parallel -j ${num_processadores} -- < $arq" >> comandos.sh
done

chmod +x comandos.sh # Torna o arquivo executável.

nohup ./comandos.sh &
```

Depois, basta monitorar o arquivo `nohup` com o comando:

`tail -f nohup &`

3 Outra forma de utilizar o parallel. Neste exemplo, será feito o uso do operador mergetime para juntar vários dias de um determinado ano.

```bash
#!/bin/bash

for ano in $(seq 2010 2018)
do
    echo "cdo -s -O mergetime RF.${ano}????00.nc tmp01.RF.${ano}.nc" >> lista.txt
done

nohup parallel -j 12 < lista1.txt > lista1.log

```

O valor `12` veio a partir do comando `nproc` que digita-se no terminal. O valor obtido divide-se por 2.

Por exemplo, ao digitar o `nproc` foi retornado o valor `24`, e 24/2 = 12. 

Este número representa o número de núcleos do seu computador. 

**Se você usar todos os núcleos, é bem provável que a sua máquina possa travar.**

O arquivo `lista.txt` terá o seguinte conteúdo.

```
cdo -s mergtime RF.2010????00.nc tmp01.RF.2010.nc
cdo -s mergtime RF.2011????00.nc tmp01.RF.2011.nc
cdo -s mergtime RF.2012????00.nc tmp01.RF.2012.nc
cdo -s mergtime RF.2013????00.nc tmp01.RF.2013.nc
cdo -s mergtime RF.2014????00.nc tmp01.RF.2014.nc
cdo -s mergtime RF.2015????00.nc tmp01.RF.2015.nc
cdo -s mergtime RF.2016????00.nc tmp01.RF.2016.nc
cdo -s mergtime RF.2017????00.nc tmp01.RF.2017.nc
cdo -s mergtime RF.2018????00.nc tmp01.RF.2018.nc
```
Qual a vantagem de se fazer isso? Em vez de gerar um loop para cada ano, os 9 comandos serão executados de uma vez só, economizando tempo de máquina.

### Localizar onde está sendo executado um comando

O `pwdx` é nativo do Linux e é excelente para identificar qual o diretório que um determinado processo foi executado.

+ Sintaxe: `sudo pwdx PID`
+ O `PID` (Process Identifier) é um número de identificação que o sistema fornece quando se executa um processo. Por exemplo, ao executar um programa, o mesmo gerará um PID. 
+ Como saber o PID de um processo? Basta utilizar o `htop`, e verificar a primeira coluna desse comando.
    + Para instalar o `htop` basta digitar: `sudo apt install htop`
+ Exemplo de uso:
    + `sudo pwdx 3859`
    + Será retornado o diretório do processo PID: 3859.
    + Exemplo de retorno: `/mnt/produtos/meteorologia`
