# Proyecto Filogenias mitocondriales de Dendrobatidae
* Integrante: Doménica Valenzuela

## Propósito del programa
* El programa a desarrollarse pretende comparar dos filogenieas de miembros del género Dendrobates de la familia Dendrobatidae, una de ellas será creada a patir del gen mitocondrial Cytb y la otra del gen 16S rNA que han pasado por un alineamiento y control de calidad.
  
## Foto
![rana](https://upload.wikimedia.org/wikipedia/commons/9/9e/Dendrobates.tinctorius.7037.jpg)

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
  

# Pasos a seguir y lista de comandos
* Descargar las secuencias genéticas usando esearch (genes Cytb y 16S)
** Para género Dendrobates
  esearch -db nuccore -query "Cytb AND Dendrobates[ORGN]" | efetch -format fasta > Cytb_Dendro.fasta
  esearch -db nuccore -query "16S AND Dendrobates[ORGN]" | efetch -format fasta > C16S_Dendro.fasta
** Para grupo externo
  esearch -db nuccore -query "Cytb AND Allobates femoralis" | efetch -format fasta > Cytb_Allo.fasta
  cat Cytb_Dendro.fasta
  nano Cytb.Allo.fasta (archivo con una sola secuencia)
  
  esearch -db nuccore -query "16S AND Allobates femoralis[ORGN]" | efetch -format fasta > 16S_Allo.fasta
  cat 16S_Dendro.fasta
  nano 16S.Allo.fasta (archivo con una sola secuencia)
  
* Juntar los fasta de Dendrobates y el grupo externo
  cat Cytb_Dendro.fasta Cytb.Allo.fasta > Cytb.final.fasta
  cat 16S_Dendro.fasta 16S.Allo.fasta > 16S.final.fasta
  
* Alinear las secuencias
  Importante: tener muscle3.8.31_i86linux64 en la carpeta
 for mito in *_final.fasta
> do ./muscle3.8.31_i86linux64 -in $mito -out muscle_$mito -maxiters 1 -diags
> done
  
* Modificar nos nombres de los genes con ATOM para dejar solo Género y Especie
  Find: ^>.*?\s([A-Z][a-z]+ [a-z]+).*
  Replace: >$1_$2
  Guardar docimentos como muscle_16S_atom.fasta y muscle_Cytb_atom.fasta

*Pasar archivos a Hoffman
1. Abrir terminal
cd Desktop/
scp dechavez@hoffman2.idre.ucla.edu:/u/scratch/d/dechavez/Bioinformatica-PUCE/RediseBio/DomenicaVP/Final/GENES/Mitocondriales/muscle_CYTB_atom_.fasta.treefile .
scp dechavez@hoffman2.idre.ucla.edu:/u/scratch/d/dechavez/Bioinformatica-PUCE/RediseBio/DomenicaVP/Final/GENES/Mitocondriales/muscle_16S_atom_.fasta.treefile .

* Eliminar secuencias repetidas
awk '/^>/ {especie=substr($0,2); if(!(especie in seen)){seen[especie]=1; print $0; flag=1} else {flag=0}} !/^>/ {if(flag) print}' muscle_16S_atom.fasta > muscle_16S_atom_filtrado.fasta

awk '/^>/ {especie=substr($0,2); if(!(especie in seen)){seen[especie]=1; print $0; flag=1} else {flag=0}} !/^>/ {if(flag) print}' muscle_CYTB_atom.fasta > muscle_CYTB_atom_filtrado.fasta

* Reconstrucción filogenética con Iq-tree
  module av iqtree
  module load iqtree/2.2.2.6
  for filogeniam in muscle_*_filtrado.fasta; do iqtree2 -s $filogeniam; done
  
*Abrir los archivos en Figtree



