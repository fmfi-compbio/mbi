---
title: "Cvičenia pre biológov: Ukážka práce v Linuxe"
---

* TOC
{:toc}

## Príprava

- Prihláste sa na server podľa pokynov.
- Potom spúšťajte jednotlivé príkazy podľa pokynov nižšie.
- Odporúčame príkazy kopírovať myšou (v internetovom prehliadači
  vysvietiť, stlačiť Ctrl-C, v konzole Ctrl-Shift-V)
 
```bash
# riadky začínajúce mrežou # sú komentáre, netreba ich spúšťať

# Dôležité: v príkazoch nižšie xx nahraďte vašimi iniciálkami, napr. bb
mkdir xx
cd xx
# príkaz mkdir (make directory) vytvoril priečinok
# príkaz cd (change directory) zmenil váš aktuálny priečinok na tento nový

# v konzole by ste mali mať user@server:~/xx$
# kde xx je číslo vašej skupiny, napr. 01

# stiahneme si súbor s dátami zo stránky
wget http://compbio.fmph.uniba.sk/vyuka/mbi-data/cb12.zip
# rozzipujeme ho
unzip cb12.zip
```

## Skladanie genómov, mapovanie čítaní, zarovnanie

```bash
# prejdeme na priečinok s prvou časťou ohľadom sekvenovania
cd 1-seq

# ls vypíše zoznam súborov v priečinku
ls
# ls -l vypíše dlhšiu informáciu (long)
ls -l
# ls -lSh usporiada súbory podľa veľkosti (Size) a veľkosti vypíše priateľskejšie pre ľudí (human)
ls -lSh

# mali by sme vidieť kúsok sekvencie z E.coli (prípona .fasta)
# a 2 súbory zo sekvenovania prístrojom Illumina Miseq  (prípona .fastaq.gz)
# tieto súbory obsahujú čítania z vyššie uvedeného kúsku genómu


# ideme skladať genóm, bude to trvať dlho, preto to chceme spustiť na pozadí
# aby sme mohli medzitým robiť niečo iné
screen # stlačte Enter
# spustite skladanie programom spades
spades.py -t 1 -m 1 --pe1-1 miseq_R1.fastq.gz --pe1-2 miseq_R2.fastq.gz -o spades > spades.log
# stlačte naraz Ctrl-a potom d
# spades teraz beží na pozadí

# príkaz top zobrazí bežiace procesy
# ukončíte ho stlačením q (quit)
top

# príkaz less umožňuje prezerať si obsah textového súboru
# aj príkaz less ukončíte stlačením q, šípkami sa pohybujete po súbore
less ref.fasta
# čítania sú komprimované, preto namiesto less použijeme zless
zless miseq_R1.fastq.gz
# tieto príkazy spočítajú počet riadkov - ako z toho zistíme počet čítaní?
zcat miseq_R1.fastq.gz | wc -l 
zcat miseq_R2.fastq.gz | wc -l 

# keď spades skončí, vrátime sa do screen a ukončíme ho
screen -r
# exit ukončí screen
exit

# spades dal výstup do podpriečinku spades, pozrime si ho
ls spades
# skopírujeme si hlavný výsledok do nášho priečinka (cp = copy)
cp -ip spades/contigs.fasta spades.fasta
less spades.fasta
# pozrime si hlavičky jednotlivých sekvencií vo fasta súbore
grep '>' spades.fasta

# programom last si spravíme dotplot referencia vs. naše skladanie
# 1) vytvorenie indexu pre referenciu
lastdb ref.fasta ref.fasta 
# 2) samotné zarovnanie
lastal -f TAB ref.fasta spades.fasta > aln.tab
# 3) vytvorenie obrázku s dotplotom
last-dotplot aln.tab aln.png

# a ešte dotplot referencia vs. referencia
# 2) samotné zarovnanie (index už máme)
lastal -f TAB ref.fasta ref.fasta > aln2.tab
# 3) vytvorenie obrázku s dotplotom
last-dotplot aln2.tab aln2.png

# pozrieme si dotploty programom eog
eog aln.png &
eog aln2.png &


# zarovnajme čítania k referenčnému genómu v 4 krokoch
# 1) indexovanie fasta súboru
bwa index ref.fasta
# 2) samotné zarovnávanie čítaní programom bwa
bwa mem ref.fasta miseq_R1.fastq.gz miseq_R2.fastq.gz > ref-miseq.sam
# 3) zmeníme textový sam formát na binárny bam formát
samtools view -S -b ref-miseq.sam | samtools sort - -o ref-miseq.bam
# 4) vytvoríme index bam súboru
samtools index ref-miseq.bam

# pozrime sa na zoznam súborov od najnovšieho po najstarší
ls -lth
# sam súbor so zarovnaniami sa dá pozrieť, ale nie je veľmi prehľadný
less ref-miseq.sam


# vytvoríme aj zarovnanie nášho poskladaného genómu k referencii vo formáte bam
samtools faidx ref.fasta
lastal ref.fasta spades.fasta -E1e-20 | maf-convert sam > ref-spades.sam
samtools view -S -b -t ref.fasta.fai ref-spades.sam | samtools sort - -o ref-spades.bam
samtools index ref-spades.bam

# výsledky si zobrazíme v grafickom prehliadači igv 
# obdoba genome browsera, ktorú si môžete nainštalovať na vašom počítači
# POZOR: POTREBUJE VEĽA PAMÄTE, SPUSTÍME IBA JEDEN NARAZ
igv -g ref.fasta
# pomocou Menu->File->Load from File otvorte ref-spades.bam a ref-miseq.bam
# pozrime si región ecoli-frag:224,000-244,000
#   Vidíte jednotlivé kontigy? Sedí tento pohľad s dotplotom? 
# a potom bližšie ecoli-frag:227,300-227,600
#   Všimnite si sekvenačné chyby, t.j. rozdiely medzi referenciou a čítaniami.
```

## Hľadanie génov, RNA-seq

Nerobili sme.

```bash
# v druhom cvičení si vyskúšame hľadanie génov
# najskôr sa presuňme do druhého priečinku
cd ../2-genes

# pozrime si, aké máme súbory
ls -lSh
# mali by sme mať kúsok referenčného genómu huby Emericella (staršie Aspergillus) nidulans 
# fastq súbor s čítaniami z RNA-seq pre tento kúsok referencie
# gff súbor s anotáciou génov z databázy

# spustíme hľadač génov Augustus 2x:
# raz s parametrami priamo pre E.nidulans a raz s parametrami pre ľudský genóm
augustus --species=anidulans ref2.fasta > augustus-anidulans.gtf
augustus --species=human ref2.fasta > augustus-human.gtf

# RNA-seq zarovnáme k sekvencii nástrojom STAR(podporuje intróny)
STAR --runMode genomeGenerate --genomeDir ref-index --genomeFastaFiles ref2.fasta  --genomeSAindexNbases 6
STAR --genomeDir ref-index --alignIntronMax 10000 --readFilesIn rnaseq.fastq  --outFileNamePrefix rnaseq-star.

samtools view -S -b rnaseq-star.Aligned.out.sam | samtools sort - -o rnaseq.bam
samtools index rnaseq.bam

# predikcie génov a RNA-seq si pozrieme v igv
igv -g ref2.fasta
# v igv si otvorte annot.gff, augustus-anidulans.gtf, augustus-human.gtf, rnaseq.bam
# - ktoré parametre Augustusu dali presnejšie predpovede (za predpokladu, že anotácia je správna)
# - pozrite si zblízka druhý gén sprava, ktorý má intróny i vysokú expresiu v RNA-seq 
#   mali by ste vidieť čítania podporujúce intróny
```

Ako by sme zistili funkciu génu, ktorý sme si pozerali?

Nejaké odkazy
* https://www.ncbi.nlm.nih.gov/nuccore/XM_659165.1?report=genbank
* https://www.ncbi.nlm.nih.gov/gene/2870422
* https://www.uniprot.org/uniprotkb/P28344/entry
