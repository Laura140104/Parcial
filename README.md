# Parcial 1
## primer punto:
### cargar los archivos:
_se descargaron los archivos fasta y luego se concatenaron, formando un solo archivo, luego se paso ese unico archivo al cluster_

```cat*.fasta```

```scp -i /home/laura_cat/llave/bio.pt.pem -P 53841 datos_ballenas  bio.pt@loginpub-hpc.urosario.edu.co:/home/bio.pt/data/Parcial1/parcial_laura_gracia```

### luego se cambiaron los nombres de las seciuencias con expresiones regulares:

_los nombre se cambiaron para que >AB201258.1 Balaenoptera edeni mitochondrial DNA, complete genome, isolate:NSMT-33622 fuera mucho mas corto y quedara asi >AB201258.1_Balaenoptera_edeni_

 ```sed -E 's/>([A-Z0-9]+\.[0-9]+) ([A-Za-z]+) ([A-Za-z]+).*/>\1_\2_\3/' ballenas_concatenadas.fasta > ballenas_renombradas.fast```
  ### Se creo una base de datos tipo Blastn con la lista de secuencias descargadas

  ```makeblastdb -in ballenas_renombradas.fasta -dbtype nucl -out ballenas_db -parse_seqids  ```

  ```blastn -query query.fasta -db ballenas_db -out blast_results.txt -outfmt 6```
  ### Descripcion del blast

  ## Segundo Punto:
  _se concatenaron los archivos de las secuencias renombradas y la query.fasta en un solo archio y se guardo en una nueva carpeta_

  ```cat ballenas_renombradas.fasta query.fasta > union.fasta```

  ```cp union.fasta concatenado_ballenas_query```

  _luego se realizó un alineamiento múltiple usando el alineador MUSCLE_

  ```module load muscle/3.8.31  ```
    ```muscle -in union.fasta -out alineamiento_muscle.fasta```


_luego se realizó un árbol de máxima verosimilitud bajo el modelo GTR+I+G y con un valor de bootstrap de 1000_

```module load iqtree/1.6.12```

```iqtree -s alineamiento_muscle.fasta -m GTR+I+G -bb 1000 -nt AUTO -pre arbol_ballenas```

![Caos](https://github.com/Laura140104/Parcial/blob/main/arbol%20parcial.png)


_seq1 es el outgroup_

## Tercer punto:
