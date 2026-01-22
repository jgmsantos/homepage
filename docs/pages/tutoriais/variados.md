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
5 Loop ao longo das previsões do modelo GFS.

```bash
#!/bin/bash

# Ajuste os parâmetros abaixo de acordo com as suas necessidades.
PREV=0    # Horário que começa a previsão de interesse (f000).
PRAZO=384 # Quantidade de previsões em horas desejadas. São 384 horas.
dPREV=3   # Intervalo temporal em horas entre cada previsão desejada.

while [[ ${PREV} -le ${PRAZO} ]]
do
    fcst=`echo ${PREV} | awk '{printf("%.3d",$1)}'`
    
    # Adicione suas instruções.
    echo "Arquivo: gfs.t00z.pgrb2.0p25.${fcst}"
    
    let PREV=${PREV}+${dPREV}
done

# A saída será da seguinte forma:

# Arquivo: gfs.t00z.pgrb2.0p25.000
# Arquivo: gfs.t00z.pgrb2.0p25.003
# Arquivo: gfs.t00z.pgrb2.0p25.006
# Arquivo: gfs.t00z.pgrb2.0p25.009
# Arquivo: gfs.t00z.pgrb2.0p25.012
# Arquivo: gfs.t00z.pgrb2.0p25.015
# Arquivo: gfs.t00z.pgrb2.0p25.018
# Arquivo: gfs.t00z.pgrb2.0p25.021
.
.
.
# Arquivo: gfs.t00z.pgrb2.0p25.381
# Arquivo: gfs.t00z.pgrb2.0p25.384
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

Neste exemplo, será feito o uso do operador mergetime para juntar vários dias de um determinado ano.

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

**Se você usar todos os núcleos, mas é bem provável que a sua máquina trave.**

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

### Configurar OneDrive para receber arquivos de uma máquina externa

Imagina transferir arquivos de uma máquina externa para o OneDrive para ser consumido por alguma aplicação. Pois é, o rclone realiza esta tarefa.

#### Informações sobre o rclone

[https://rclone.org/onedrive/](https://rclone.org/onedrive/)

#### Serviços suportados (principais)

Basta configurar cada serviço como um remote.

* Google Drive
* Dropbox
* OneDrive
* Box
* Mega
* Amazon S3
* Google Cloud Storage
* SFTP/FTP
* WebDAV
* Backblaze B2
* e muitos outros

#### Configurar o usuário remote

Vá para o seu home digitando ```cd``` no seu terminal Linux. Em seguida, digite ```pwd``` apenas para certificar que esta no home, deve aparecer algo assim ```/home/<usuario>```, em que ```<usuario>``` é o nome do usuário da sua máquina. Exemplo: ```/home/gui```.

Agora, digite ```rclone listremotes``` para ver quais remotos já existem. Caso o seu remoto não esteja lista será necessário criar um. Os passos abaixo realizam esta tarefa.

Se quiser criar um novo remote (por exemplo, para o OneDrive da conta gui), digite no terminal do Linux:

```bash
rclone config
```

No menu interativo do rclone que será aberto responda as perguntas abaixo:

1. Escolha `n` (New remote)
2. Em **Name**, escolha um nome para o remote, por exemplo: `OneDriveGui`. Este nome é definido pelo usuário.
3. Em **Storage**, escolha o número 21 que corresponde a **Microsoft OneDrive**
4. No campo **client_id** basta `pressionar enter`
5. No campo **client_secret** basta `pressionar enter`
6. No campo **Edit advanced config** digite `n`
7. No campo **Remote config** digite `y`
8. Será aberto um nagevador (será informado um link no terminal), entre com as suas credenciais (usuário e senha). Dependendo da conta, se for corporativa, será necessário falar com o administrador do sistema para autorizar o acesso.
9. No terminal, será mostrado o campo **Choose a number from below, or type in an existing value**. Digite o número `1`.

10. Será mostrada a mensagem

Found 1 drives, please select the one you want to use: 
`0: OneDrive (business)`

Digite o número `0`

No final, confirme e saia.

`Is that okay?`

Digite `y`

> 🚨 **Aviso importante**  
> Provavelmente, será mostrado um erro, mas ele pode ser ignorado.

E por fim,

`Yes this is OK`

Digite `y`

Depois, digite ```rclone listremotes``` para listar os remotes existentes.

```bash
OneDriveGui:
```

Depois disso, você poderá usar o novo remote no Linux, por exemplo. Copiar do computador para uma pasta no OneDrive.

```bash
rclone copy /caminho/arquivos OneDriveGui:output/modelos/gfs
```

Onde: 

* `/caminho/arquivos` é o arquivo que você deseja enviar
* `output/modelos/gfs` é a pasta do OneDrive que você deseja salvar os seus arquivos. Basta colocar apenas o caminho relativo da pasta.

> Observação: o rclone roda **no Linux** e fala diretamente com a nuvem do OneDrive.