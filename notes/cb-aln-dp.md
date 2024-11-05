---
title: "Cvičenia pre biológov: Dáta k programom water a needle"
---

Sekvencie a zarovnania k cvičeniu pre biológov: programy water a needle

## Proteínové sekvencie

  - <https://genome.compbio.fmph.uniba.sk/data/magCapA5/proteins/MCA_00027_1.html>
  - <https://www.uniprot.org/uniprot/P38222#sequences>


```
    >sp|P50520|VPS34_SCHPO Phosphatidylinositol 3-kinase vps34 OS=Schizosaccharomyces pombe (strain 972 / ATCC 24843) GN=vps34 PE=2 SV=2
    MDRLVFSYCPSSKVTARFLVKFCFIEYQDSQEPCICTIQLFSGNESGSLMQKCFVSKIPN
    KSLLPTELSKISTHEWLDFGVTVSELSLNAKFVVSAWKPSFNDEEVYEFVGCTTYRLFDE
    NNLLRQGLQKIPLQTSKEIKKYSPTSLELEQVKEINRLDGLLLKLQLGDVPSVNWLDDIS
    FGKIKDFRSKHMSLVTIPILYLDFLQFSFPVVFQRSYYPKSENRVYYSSFDLELNLDSPA
    ELKHRRLVRSQRNGPLDKDLKPNSKIRKELESILSYPPSEELSLEEKDLIWKFRFYLTRN
    KKAMTKFLKSVVWTDSSEVNQALSLLDSWTEIDIDDALELLSPSFVHPKVRAYAVSRLET
    ASNEELLLYLLQLVQALRYDNPISSDERFQPSPLALFLVNRAISSPSIGNDLYWYLVVEI
    EDEPVSKLFSSVMFLFQKELSKSVEGRLIRETLSAQAKFVEKLLRISKSVQSFRGTRLKK
    IEYLKVLLEDHKYHLLDFHALPLPLDPSVNIVGIIPDACTVFKSTMQPLRLLFKCQDGSK
    YPIIFKNGDDLRQDQLVIQILTLMDKLLKKEKLDLHLKPYRILATGPTHGAVQFVPSKTL
    ATILAEYHGSVLAYLRENNPDDGLNSANYGIDPVAMDNYVRSCAGYCVITYLLGVGDRHL
    DNLLITKDGHFFHADFGYILGRDPKLFSPAMKLSKEMVEGMGGYNSPFYQQFKSYCYTTF
    TALRKSSNLILNLFSLMVDANIPDIKFDKEKVVYKVKERFCLQMSESDAIKYFEQLINDS
    VSALFPQIIDRMHNLAQYMRS

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

## Globálne zarovnanie s nulovou penaltou medzier na koncoch sekvencie

```
    ########################################
    # Program: needle
    # Rundate: Thu 14 Oct 2021 15:05:37
    # Commandline: needle
    #    -auto
    #    -stdout
    #    -asequence emboss_needle-I20211014-151156-0154-17922699-p1m.asequence
    #    -bsequence emboss_needle-I20211014-151156-0154-17922699-p1m.bsequence
    #    -datafile EBLOSUM62
    #    -gapopen 10.0
    #    -gapextend 0.5
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
    # Length: 651
    # Identity:     177/651 (27.2%)
    # Similarity:   282/651 (43.3%)
    # Gaps:         155/651 (23.8%)
    # Score: 608.0
    # 
    #
    #=======================================
    
    MCA_00027_1        1 ----MEDSLMERYQSFV----GWMLDNKIQFSSLLTIRESPLGGIGVFAR     42
                             .:|. :.|...||    |...|:|..      ||||||||:||||:
    RKM3_YEAST         1 MSVTFKDD-VHRILKFVANCNGRFEDSKCD------IRESPLGGLGVFAK     43
    
    MCA_00027_1       43 KKIPKSSLILMVPKNVILSPSNCSISNLLDEADFDGMLGLALAYMYERSL     92
                         ..|.:...||.:.|:.|.|.||.||:|||.::..||||.|.:|::||.::
    RKM3_YEAST        44 TDIAEGESILTLNKSSIFSASNSSIANLLCDSSIDGMLALNIAFIYETTV     93
    
    MCA_00027_1       93 GPDSL-WYEFIQTI---DHDSLISENPRFWPPEDEELLVGTELYYHTL--    136
                         ..:|. ||.|::||   |.:..::..|.||..:.:.||.||.  :.||  
    RKM3_YEAST        94 FRNSSHWYPFLRTIRIRDDEGHLNLPPSFWHADAKRLLKGTS--FDTLFD    141
    
    MCA_00027_1      137 -KVEDDDIAEVFKFDV------------------LPFLERNHLFEGQPQY    167
                          ...:::|.|.|:..|                  |...|.||    :..|
    RKM3_YEAST       142 SLAPEEEIMEGFEIAVDLAHKWNDEFGLEIPKGFLDVSEENH----EEDY    187
    
    MCA_00027_1      168 R-TLEYYRDCLVAVASRAFDVDVYHGLALVPGACLFNHSI-DESVHFEVV    215
                         . .||.:......::||.|::|.||..||||.|.||||.: |..:.|..:
    RKM3_YEAST       188 NLKLEKFISVAYTLSSRGFEIDAYHETALVPIADLFNHHVSDPDLKFVSL    237
    
    MCA_00027_1      216 SQVCPLCGSPDFCDHLANRLGNHSESDEEEEEEEVDELNEDGFSSGFDYS    265
                         ..||..||.||.|.||.        ::|..|.|.:|:....         
    RKM3_YEAST       238 YDVCDKCGEPDMCKHLI--------AEEYLEAENLDKNMPK---------    270
    
    MCA_00027_1      266 DSESDSDGFEDIEEEEEEDGIIGSEETTIPISEDSDKKTSNEQKSEEQEA    315
                                              :.|.||.: |.||..|...|:.:.|....
    RKM3_YEAST       271 ---------------------VASMETRV-IDEDLIKSLENDLEKEYSNV    298
    
    MCA_00027_1      316 EKYQYEEDDEDEADYDEPQDTCDIVTIKSVHKGNEVFNTYGELSNHHLAS    365
                         .    ...::|:...:.|.:..|:|....|.:|.|:||:||||||..|.:
    RKM3_YEAST       299 T----ANIEDDDGGIENPDECVDLVLKNDVAQGQEIFNSYGELSNVFLLA    344
    
    MCA_00027_1      366 RYGFAIWDNKYETVGLSPEIRQYI-SENNLMERQEWWSIYFYEALFGIRK    414
                         ||||.:.:|:|:.|.|.|:..:.: .|....|:.:|||    :...|:..
    RKM3_YEAST       345 RYGFTVPENQYDIVHLGPDFMKILKKEEKYQEKVKWWS----QVGHGLFS    390
    
    MCA_00027_1      415 DEWAEIEESEDEDDEG------------SEDSEEENSIPPSPESISWEDE    452
                         ..:|::.:.::||::|            ||:.|||.....|.|  ||..:
    RKM3_YEAST       391 AWYAQMRQEDEEDEDGQAKSDNLSDDIESEEEEEEEEGDDSLE--SWLSQ    438
    
    MCA_00027_1      453 AYLTNSGAPSEGLSKLIRILSMSDSDFEALKSQFERDVFTSKLLPS-STL    501
                         .|:.:||.||.....|..:|:::...:|:|        |:.|..|. |..
    RKM3_YEAST       439 LYIDSSGEPSPSTWALANLLTLTAVQWESL--------FSKKATPHISDS    480
    
    MCA_00027_1      502 VFNEKSLILLKALVNLRLQRYKDGMLTSSQI-------KDLITKLSDNQK    544
                         :.||:.|..|....|...::....:|...|:       ...||..:.:..
    RKM3_YEAST       481 IVNEEKLPFLAKKDNPHSKKLLSNLLKEKQLPCIKGDNSSKITSATKSML    530
    
    MCA_00027_1      545 KNSRQILALTIKGTEKIVLEKSMQWIAQLEKKKKRKPSNSNHKKPNSRKL    594
                         :|:|     |:..:|..:|::.::.::                       
    RKM3_YEAST       531 QNAR-----TLVQSEHNILDRCLKRLS-----------------------    552
    
    MCA_00027_1      595 K    595
                          
    RKM3_YEAST       553 -    552
```
