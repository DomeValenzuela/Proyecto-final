# Proyecto Filogenias nuclear y mitocondrial de Dendrobatidae
* Integrante: Doménica Valenzuela

## Propósito del programa
* El programa a desarrollarse pretende comparar dos filogenieas de miembros del género Dendrobates de la familia Dendrobatidae, una de ellas será creada a patir del gen mitocondrial Cytb y la otra del gen 16S rNA que han pasado por un alineamiento y control de calidad.


## Q2. Requisitos
* Git-Bash
* Muscle
* Iq-tree
* Atom
* Figtree

## Estructura de carpetas
* Final (GENES, PROGRAMAS, SCRIPTS)
* GENES (mitocondriales)
* Mitocondriales (resultados,fasta)
* resultados (alineados, atom, trees)

## Pasos a seguir y comandos
* Descargar las secuencias del GeneBank
  TYR="TYR AND Dendrobates"
  esearch -db nucleotide -query "$TYR"  | efetch -format fasta > data/genesfasta/tyr.fasta
* Control de calidad con FASTQC
  module load fastqc
  fastqc SRR2589044_1.fastq.gz
  mkdir -p data/fastqc
  fastqc data/genesfasta/tyr.fasta -o data/fastqc
* Modificar nos nombres de los genes con ATOM para dejar solo Género y Especie y guardar estos fasta 
* Alineamiento con muscle haciendo un forloop
  for aln in *clean.fasta; do ./muscle3.8.31_i86linux64 -in $aln -out muscle_$aln; done
* Reconstrucción filogenética con Iq-tree
  module av iqtree
  module load iqtree/2.2.2.6
  iqtree2
  for filogenia in muscle_*.fasta; do iqtree2 -s $filogenia; done
  
## Q4. Sube una foto que represente tu organismo o grupo de organismo.
![alt text](https://multimedia20stg.blob.core.windows.net/especiesreduced/DSC07211.jpg)

# Lista de comandos
