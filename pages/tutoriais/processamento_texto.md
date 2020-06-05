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

+ Transforma coluna para linha: 

`sed 's/  /\n/g' seu.arquivo.texto > out.txt`

### Dicas utilizando o AWK

+ Imprime as linhas pares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 != {print}' > out.txt`

+ Imprime as linhas ímpares de um arquivo: 

`cat -n seu.arquivo.texto | awk '$1 % $2 == {print}' > out.txt`

+ Transforma coluna para linha: 

`cat seu.arquivo.texto | awk '{gsub(/  /,"\n");print}' > out.txt`

+ Deleta a coluna 1 ($1) de um arquivo texto: 

`cat seu.arquivo.texto | awk '{$1=""; print}' > out.txt`

### Dicas usando o xargs

+ Transforma linha para coluna: 

`cat seu.arquivo.texto | xargs`

### Dicas usando o find

+ Excluir arquivos vazios no diretório corrente (.): 

`find . -type f -empty | xargs rm`

+ Excluir diretório vazios no diretório corrente (.): 

`find . -type d -empty | xargs rm -rf`

### Dicas usando o shell

+ [Sams Teach Yourself Shell Programming in 24 Hours](https://drive.google.com/open?id=1b6wHLgNByMUT06tkx-o2T1EbPJs4zMpV)

+ [Advanced Bash-Scripting Guide. Autor: Mendel Cooper.](https://drive.google.com/open?id=16GPHZQK2Bsd_FmfPx_MirrMcEwwMuZYc)

+ [Linux Shell Scripting Tutorial v1.05r3 A Beginner's handbook. Autor: Vivek G. Gite.](https://drive.google.com/open?id=1HIveI42BcDEN4SE7XgzS6easdEmgN83n)

+ [Manipulating Strings](https://drive.google.com/file/d/1-_c7zeNH_aI2KQhZ4ZufdvBd41jzwiqG/view?usp=sharing)

+ [String Operations in Shell](https://drive.google.com/file/d/152p14yGH4NpB6JCqqD5unGSl8H9J2lv_/view?usp=sharing)

#### Links interessantes sobre shell

+ [Softpanorama](http://www.softpanorama.org/Scripting/Shellorama/String_operations/index.shtml)
	
+ [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/string-manipulation.html)