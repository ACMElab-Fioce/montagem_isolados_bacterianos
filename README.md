# Tutorial de montagem e visualização de contigs de bactérias utilizando uma junção entre a pipeline BACTOPIA e MUMMER:

### Link do github do Bactopia: https://github.com/bactopia/bactopia
### Link do github do Mummer: https://github.com/mummer4/mummer
Antes de tudo, acesse o computador via ssh.

### Criação do ambiente:
```
conda create --name bactoflow
```

--------------------------------
## 1. BACTOPIA

### 1) Instalação:
```
conda install -c bioconda bactopia
```

### 2) Acessando os datasets:
bactopia datasets

### 3) Execução:
### Paired-end:

```
bactopia --R1 R1.fastq.gz --R2 R2.fastq.gz --sample SAMPLE_NAME \
         --datasets datasets/ --outdir OUTDIR
```

### Single-End:

```
bactopia --SE SAMPLE.fastq.gz --sample SAMPLE --datasets datasets/ --outdir OUTDIR
```

### Arquivo run_bactopia.sh:
```
conda activate bactopia

for r1 in *_R1_001.fastq.gz; do
    r2=${r1/_R1_/_R2_}  # Substitui R1 por R2 para obter o nome correto do par
    sample=$(echo $r1 | cut -d'_' -f1-3)  # Define o nome da amostra baseado no nome do arquivo
    outdir="${sample}-RESULTS"

    bactopia \
        --r1 "$r1" \
        --r2 "$r2" \
        --sample "$sample" \
        --coverage 100 \
        --genome_size 4600000 \
        --outdir "$outdir" \
        --max_cpus 2
done
```
--------------------------------
## 2. MUMMER
### 1) Instalação:
```
conda install -c bioconda mummer
```

### 2) Execução para alinhar contigs contra a referência:
```
nucmer -p prefixo_saida referencia.fasta contigs.fasta
```

### 3) Visualização do alinhamento:
```
show-coords -rcl prefixo_saida.delta > alinhamento.coords
```

--------------------------------
### Os dados foram analisados utilizando o Notebook *grafico_mummer.ipynb*
![SA-5-ITB-COVIDSEQ](https://github.com/user-attachments/assets/86b04cbd-5c6c-4605-ab3f-49f0328a9ae1)



