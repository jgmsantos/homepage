Miniconda
============

## Instalação do miniconda no Linux por meio do site oficial:

[https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

## Baixar o instalador

+ Digitar no seu terminal Linux o comando:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

+ Executar o instalador

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

Basta aceitar todas as opções que vão aparecendo no processo de instalação.

## Criar um ambiente

O `<nome_ambiente>` é o nome do ambiente que se deseja criar.

```bash
conda create --name <nome_ambiente>
```

## Desativar um ambiente

```bash
conda deactivate
```
## Ativar um ambiente

```bash
conda activate <nome_ambiente>
```
## Remover um ambiente

```bash
conda env remove --name <nome_ambiente>
```

## Atualização de um pacote
```bash
conda update <nome_pacote>
```

## Instalação de pacotes

### Instalação do NCL

```bash
conda install -c conda-forge ncl
```

### Instalação do gdal

```bash
conda install -c conda-forge gdal
```

### Instalação do CDO:

```bash
conda install -c conda-forge cdo
```

### Instalação do imagemagick

```bash
conda install -c conda-forge imagemagick
```

### Instalação do ncview
conda install -c conda-forge ncview

### Instalação do htop

```bash
conda install -c conda-forge htop
```

### Instalação do parallel

```bash
conda install -c conda-forge parallel
```

### Instalação do nco

```bash
conda install -c conda-forge nco
```