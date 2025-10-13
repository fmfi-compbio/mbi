---
title: "Cvičenia pre biológov: Dáta k programom water a needle"
---

Sekvencie a zarovnania k cvičeniu pre biológov: programy water a needle

## Proteínové sekvencie

  - <https://genome.compbio.fmph.uniba.sk/data/magCapA5/proteins/MCA_00027_1.html>
  - <https://www.uniprot.org/uniprot/P38222#sequences>


```
>MCA_00027_1
MEDSLMERYQSFVGWMLDNKIQFSSLLTIRESPLGGIGVFARKKIPKSSLILMVPKNVIL
SPSNCSISNLLDEADFDGMLGLALAYMYERSLGPDSLWYEFIQTIDHDSLISENPRFWPP
EDEELLVGTELYYHTLKVEDDDIAEVFKFDVLPFLERNHLFEGQPQYRTLEYYRDCLVAV
ASRAFDVDVYHGLALVPGACLFNHSIDESVHFEVVSQVCPLCGSPDFCDHLANRLGNHSE
SDEEEEEEEVDELNEDGFSSGFDYSDSESDSDGFEDIEEEEEEDGIIGSEETTIPISEDS
DKKTSNEQKSEEQEAEKYQYEEDDEDEADYDEPQDTCDIVTIKSVHKGNEVFNTYGELSN
HHLASRYGFAIWDNKYETVGLSPEIRQYISENNLMERQEWWSIYFYEALFGIRKDEWAEI
EESEDEDDEGSEDSEEENSIPPSPESISWEDEAYLTNSGAPSEGLSKLIRILSMSDSDFE
ALKSQFERDVFTSKLLPSSTLVFNEKSLILLKALVNLRLQRYKDGMLTSSQIKDLITKLS
DNQKKNSRQILALTIKGTEKIVLEKSMQWIAQLEKKKKRKPSNSNHKKPNSRKLK

>RKM3_YEAST
MSVTFKDDVHRILKFVANCNGRFEDSKCDIRESPLGGLGVFAKTDIAEGESILTLNKSSI
FSASNSSIANLLCDSSIDGMLALNIAFIYETTVFRNSSHWYPFLRTIRIRDDEGHLNLPP
SFWHADAKRLLKGTSFDTLFDSLAPEEEIMEGFEIAVDLAHKWNDEFGLEIPKGFLDVSE
ENHEEDYNLKLEKFISVAYTLSSRGFEIDAYHETALVPIADLFNHHVSDPDLKFVSLYDV
CDKCGEPDMCKHLIAEEYLEAENLDKNMPKVASMETRVIDEDLIKSLENDLEKEYSNVTA
NIEDDDGGIENPDECVDLVLKNDVAQGQEIFNSYGELSNVFLLARYGFTVPENQYDIVHL
GPDFMKILKKEEKYQEKVKWWSQVGHGLFSAWYAQMRQEDEEDEDGQAKSDNLSDDIESE
EEEEEEEGDDSLESWLSQLYIDSSGEPSPSTWALANLLTLTAVQWESLFSKKATPHISDS
IVNEEKLPFLAKKDNPHSKKLLSNLLKEKQLPCIKGDNSSKITSATKSMLQNARTLVQSE
HNILDRCLKRLS
```

## Lokálne zarovnanie

```
    ########################################
    # Program: water
    # Rundate: Thu 14 Oct 2021 15:08:56
    # Commandline: water
    #    -auto
    #    -stdout
    #    -asequence emboss_water-I20211014-150851-0938-20653107-p2m.asequence
    #    -bsequence emboss_water-I20211014-150851-0938-20653107-p2m.bsequence
    #    -datafile EBLOSUM62
    #    -gapopen 10.0
    #    -gapextend 0.5
    #    -aformat3 pair
    #    -sprotein1
    #    -sprotein2
    # Align_format: pair
    # Report_file: stdout
    ########################################
    
    #=======================================
    #
    # Aligned_sequences: 2
    # 1: MCA_00027_1
    # 2: RKM3_YEAST
    # Matrix: EBLOSUM62
    # Gap_penalty: 10.0
    # Extend_penalty: 0.5
    #
    # Length: 588
    # Identity:     170/588 (28.9%)
    # Similarity:   270/588 (45.9%)
    # Gaps:         116/588 (19.7%)
    # Score: 611.0
    # 
    #
    #=======================================
    
    MCA_00027_1       29 IRESPLGGIGVFARKKIPKSSLILMVPKNVILSPSNCSISNLLDEADFDG     78
                         ||||||||:||||:..|.:...||.:.|:.|.|.||.||:|||.::..||
    RKM3_YEAST        30 IRESPLGGLGVFAKTDIAEGESILTLNKSSIFSASNSSIANLLCDSSIDG     79
    
    MCA_00027_1       79 MLGLALAYMYERSLGPDSL-WYEFIQTI---DHDSLISENPRFWPPEDEE    124
                         ||.|.:|::||.::..:|. ||.|::||   |.:..::..|.||..:.:.
    RKM3_YEAST        80 MLALNIAFIYETTVFRNSSHWYPFLRTIRIRDDEGHLNLPPSFWHADAKR    129
    
    MCA_00027_1      125 LLVGTELYYHTL---KVEDDDIAEVFKFDV------------------LP    153
                         ||.||.  :.||   ...:::|.|.|:..|                  |.
    RKM3_YEAST       130 LLKGTS--FDTLFDSLAPEEEIMEGFEIAVDLAHKWNDEFGLEIPKGFLD    177
    
    MCA_00027_1      154 FLERNHLFEGQPQYR-TLEYYRDCLVAVASRAFDVDVYHGLALVPGACLF    202
                         ..|.||    :..|. .||.:......::||.|::|.||..||||.|.||
    RKM3_YEAST       178 VSEENH----EEDYNLKLEKFISVAYTLSSRGFEIDAYHETALVPIADLF    223
    
    MCA_00027_1      203 NHSI-DESVHFEVVSQVCPLCGSPDFCDHLANRLGNHSESDEEEEEEEVD    251
                         ||.: |..:.|..:..||..||.||.|.||.        ::|..|.|.:|
    RKM3_YEAST       224 NHHVSDPDLKFVSLYDVCDKCGEPDMCKHLI--------AEEYLEAENLD    265
    
    MCA_00027_1      252 ELNEDGFSSGFDYSDSESDSDGFEDIEEEEEEDGIIGSEETTIPISEDSD    301
                         :....                              :.|.||.: |.||..
    RKM3_YEAST       266 KNMPK------------------------------VASMETRV-IDEDLI    284
    
    MCA_00027_1      302 KKTSNEQKSEEQEAEKYQYEEDDEDEADYDEPQDTCDIVTIKSVHKGNEV    351
                         |...|:.:.|.....    ...::|:...:.|.:..|:|....|.:|.|:
    RKM3_YEAST       285 KSLENDLEKEYSNVT----ANIEDDDGGIENPDECVDLVLKNDVAQGQEI    330
    
    MCA_00027_1      352 FNTYGELSNHHLASRYGFAIWDNKYETVGLSPEIRQYI-SENNLMERQEW    400
                         ||:||||||..|.:||||.:.:|:|:.|.|.|:..:.: .|....|:.:|
    RKM3_YEAST       331 FNSYGELSNVFLLARYGFTVPENQYDIVHLGPDFMKILKKEEKYQEKVKW    380
    
    MCA_00027_1      401 WSIYFYEALFGIRKDEWAEIEESEDEDDEG------------SEDSEEEN    438
                         ||    :...|:....:|::.:.::||::|            ||:.|||.
    RKM3_YEAST       381 WS----QVGHGLFSAWYAQMRQEDEEDEDGQAKSDNLSDDIESEEEEEEE    426
    
    MCA_00027_1      439 SIPPSPESISWEDEAYLTNSGAPSEGLSKLIRILSMSDSDFEALKSQFER    488
                         ....|.|  ||..:.|:.:||.||.....|..:|:::...:|:|      
    RKM3_YEAST       427 EGDDSLE--SWLSQLYIDSSGEPSPSTWALANLLTLTAVQWESL------    468
    
    MCA_00027_1      489 DVFTSKLLPS-STLVFNEKSLILLKALVNLRLQRYKDGMLTSSQI-----    532
                           |:.|..|. |..:.||:.|..|....|...::....:|...|:     
    RKM3_YEAST       469 --FSKKATPHISDSIVNEEKLPFLAKKDNPHSKKLLSNLLKEKQLPCIKG    516
    
    MCA_00027_1      533 --KDLITKLSDNQKKNSRQILALTIKGTEKIVLEKSMQ    568
                           ...||..:.:..:|:|     |:..:|..:|::.::
    RKM3_YEAST       517 DNSSKITSATKSMLQNAR-----TLVQSEHNILDRCLK    549
```

## Globálne zarovnanie

```
    ########################################
    # Program: needle
    # Rundate: Thu 14 Oct 2021 15:13:34
    # Commandline: needle
    #    -auto
    #    -stdout
    #    -asequence emboss_needle-I20211014-151332-0965-44565643-p2m.asequence
    #    -bsequence emboss_needle-I20211014-151332-0965-44565643-p2m.bsequence
    #    -datafile EBLOSUM62
    #    -gapopen 10.0
    #    -gapextend 0.5
    #    -endweight
    #    -endopen 10.0
    #    -endextend 0.5
    #    -aformat3 pair
    #    -sprotein1
    #    -sprotein2
    # Align_format: pair
    # Report_file: stdout
    ########################################
    
    #=======================================
    #
    # Aligned_sequences: 2
    # 1: MCA_00027_1
    # 2: RKM3_YEAST
    # Matrix: EBLOSUM62
    # Gap_penalty: 10.0
    # Extend_penalty: 0.5
    #
    # Length: 650
    # Identity:     178/650 (27.4%)
    # Similarity:   282/650 (43.4%)
    # Gaps:         153/650 (23.5%)
    # Score: 588.5
    # 
    #
    #=======================================
    
    MCA_00027_1        1 MEDSL---MERYQSFV----GWMLDNKIQFSSLLTIRESPLGGIGVFARK     43
                         |..:.   :.|...||    |...|:|..      ||||||||:||||:.
    RKM3_YEAST         1 MSVTFKDDVHRILKFVANCNGRFEDSKCD------IRESPLGGLGVFAKT     44
    
    MCA_00027_1       44 KIPKSSLILMVPKNVILSPSNCSISNLLDEADFDGMLGLALAYMYERSLG     93
                         .|.:...||.:.|:.|.|.||.||:|||.::..||||.|.:|::||.::.
    RKM3_YEAST        45 DIAEGESILTLNKSSIFSASNSSIANLLCDSSIDGMLALNIAFIYETTVF     94
    
    MCA_00027_1       94 PDSL-WYEFIQTI---DHDSLISENPRFWPPEDEELLVGTELYYHTL---    136
                         .:|. ||.|::||   |.:..::..|.||..:.:.||.||.  :.||   
    RKM3_YEAST        95 RNSSHWYPFLRTIRIRDDEGHLNLPPSFWHADAKRLLKGTS--FDTLFDS    142
    
    MCA_00027_1      137 KVEDDDIAEVFKFDV------------------LPFLERNHLFEGQPQYR    168
                         ...:::|.|.|:..|                  |...|.||    :..|.
    RKM3_YEAST       143 LAPEEEIMEGFEIAVDLAHKWNDEFGLEIPKGFLDVSEENH----EEDYN    188
    
    MCA_00027_1      169 -TLEYYRDCLVAVASRAFDVDVYHGLALVPGACLFNHSI-DESVHFEVVS    216
                          .||.:......::||.|::|.||..||||.|.||||.: |..:.|..:.
    RKM3_YEAST       189 LKLEKFISVAYTLSSRGFEIDAYHETALVPIADLFNHHVSDPDLKFVSLY    238
    
    MCA_00027_1      217 QVCPLCGSPDFCDHLANRLGNHSESDEEEEEEEVDELNEDGFSSGFDYSD    266
                         .||..||.||.|.||.        ::|..|.|.:|:....          
    RKM3_YEAST       239 DVCDKCGEPDMCKHLI--------AEEYLEAENLDKNMPK----------    270
    
    MCA_00027_1      267 SESDSDGFEDIEEEEEEDGIIGSEETTIPISEDSDKKTSNEQKSEEQEAE    316
                                             :.|.||.: |.||..|...|:.:.|.....
    RKM3_YEAST       271 --------------------VASMETRV-IDEDLIKSLENDLEKEYSNVT    299
    
    MCA_00027_1      317 KYQYEEDDEDEADYDEPQDTCDIVTIKSVHKGNEVFNTYGELSNHHLASR    366
                             ...::|:...:.|.:..|:|....|.:|.|:||:||||||..|.:|
    RKM3_YEAST       300 ----ANIEDDDGGIENPDECVDLVLKNDVAQGQEIFNSYGELSNVFLLAR    345
    
    MCA_00027_1      367 YGFAIWDNKYETVGLSPEIRQYI-SENNLMERQEWWSIYFYEALFGIRKD    415
                         |||.:.:|:|:.|.|.|:..:.: .|....|:.:|||    :...|:...
    RKM3_YEAST       346 YGFTVPENQYDIVHLGPDFMKILKKEEKYQEKVKWWS----QVGHGLFSA    391
    
    MCA_00027_1      416 EWAEIEESEDEDDEG------------SEDSEEENSIPPSPESISWEDEA    453
                         .:|::.:.::||::|            ||:.|||.....|.|  ||..:.
    RKM3_YEAST       392 WYAQMRQEDEEDEDGQAKSDNLSDDIESEEEEEEEEGDDSLE--SWLSQL    439
    
    MCA_00027_1      454 YLTNSGAPSEGLSKLIRILSMSDSDFEALKSQFERDVFTSKLLPS-STLV    502
                         |:.:||.||.....|..:|:::...:|:|        |:.|..|. |..:
    RKM3_YEAST       440 YIDSSGEPSPSTWALANLLTLTAVQWESL--------FSKKATPHISDSI    481
    
    MCA_00027_1      503 FNEKSLILLKALVNLRLQRYKDGMLTSSQI-------KDLITKLSDNQKK    545
                         .||:.|..|....|...::....:|...|:       ...||..:.:..:
    RKM3_YEAST       482 VNEEKLPFLAKKDNPHSKKLLSNLLKEKQLPCIKGDNSSKITSATKSMLQ    531
    
    MCA_00027_1      546 NSRQILALTIKGTEKIVLEKSMQWIAQLEKKKKRKPSNSNHKKPNSRKLK    595
                         |:|     |:..:|..:|::.:                        ::|.
    RKM3_YEAST       532 NAR-----TLVQSEHNILDRCL------------------------KRLS    552
```
