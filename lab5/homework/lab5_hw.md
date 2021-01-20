---
title: "dplyr Superhero"
date: "2021-01-20"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.0     ✓ purrr   0.3.4
## ✓ tibble  3.0.4     ✓ dplyr   0.8.5
## ✓ tidyr   1.1.0     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("~/Desktop/BIS15LW2021_jledford/lab5/data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("~/Desktop/BIS15LW2021_jledford/lab5/data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
superhero_info <- rename(superhero_info, gender=Gender, eye_color="Eye color", race=Race, hair_color="Hair color", height="Height", publisher="Publisher", skin_color="Skin color", alignment="Alignment", weight=Weight)
superhero_info
```

```
## # A tibble: 734 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Abom… Male   green     Huma… No Hair       203 Marvel C… <NA>       bad      
##  5 Abra… Male   blue      Cosm… Black          NA Marvel C… <NA>       bad      
##  6 Abso… Male   blue      Human No Hair       193 Marvel C… <NA>       bad      
##  7 Adam… Male   blue      <NA>  Blond          NA NBC - He… <NA>       good     
##  8 Adam… Male   blue      Human Blond         185 DC Comics <NA>       good     
##  9 Agen… Female blue      <NA>  Blond         173 Marvel C… <NA>       good     
## 10 Agen… Male   brown     Human Brown         178 Marvel C… <NA>       good     
## # … with 724 more rows, and 1 more variable: weight <dbl>
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He… `Lantern Power … `Dimensional Aw…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral <- superhero_info %>% filter(alignment=="neutral")

neutral$name
```

```
##  [1] "Bizarro"         "Black Flash"     "Captain Cold"    "Copycat"        
##  [5] "Deadpool"        "Deathstroke"     "Etrigan"         "Galactus"       
##  [9] "Gladiator"       "Indigo"          "Juggernaut"      "Living Tribunal"
## [13] "Lobo"            "Man-Bat"         "One-Above-All"   "Raven"          
## [17] "Red Hood"        "Red Hulk"        "Robin VI"        "Sandman"        
## [21] "Sentry"          "Sinestro"        "The Comedian"    "Toad"
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% select("name", "alignment", "race")
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
not_human <- superhero_info %>% filter(race!="Human")

not_human$name
```

```
##   [1] "Abe Sapien"                "Abin Sur"                 
##   [3] "Abomination"               "Abraxas"                  
##   [5] "Ajax"                      "Alien"                    
##   [7] "Amazo"                     "Angel"                    
##   [9] "Angel Dust"                "Anti-Monitor"             
##  [11] "Anti-Venom"                "Apocalypse"               
##  [13] "Aqualad"                   "Aquaman"                  
##  [15] "Archangel"                 "Ardina"                   
##  [17] "Atlas"                     "Atlas"                    
##  [19] "Aurora"                    "Azazel"                   
##  [21] "Beast"                     "Beyonder"                 
##  [23] "Big Barda"                 "Bill Harken"              
##  [25] "Bionic Woman"              "Birdman"                  
##  [27] "Bishop"                    "Bizarro"                  
##  [29] "Black Bolt"                "Black Canary"             
##  [31] "Black Flash"               "Blackout"                 
##  [33] "Blackwulf"                 "Blade"                    
##  [35] "Blink"                     "Bloodhawk"                
##  [37] "Boba Fett"                 "Boom-Boom"                
##  [39] "Brainiac"                  "Brundlefly"               
##  [41] "Cable"                     "Cameron Hicks"            
##  [43] "Captain Atom"              "Captain Marvel"           
##  [45] "Captain Planet"            "Captain Universe"         
##  [47] "Carnage"                   "Century"                  
##  [49] "Cerebra"                   "Chamber"                  
##  [51] "Colossus"                  "Copycat"                  
##  [53] "Crystal"                   "Cyborg"                   
##  [55] "Cyborg Superman"           "Cyclops"                  
##  [57] "Darkseid"                  "Darkstar"                 
##  [59] "Darth Maul"                "Darth Vader"              
##  [61] "Data"                      "Dazzler"                  
##  [63] "Deadpool"                  "Deathlok"                 
##  [65] "Demogoblin"                "Doc Samson"               
##  [67] "Donatello"                 "Donna Troy"               
##  [69] "Doomsday"                  "Dr Manhattan"             
##  [71] "Drax the Destroyer"        "Etrigan"                  
##  [73] "Evil Deadpool"             "Evilhawk"                 
##  [75] "Exodus"                    "Faora"                    
##  [77] "Fin Fang Foom"             "Firestar"                 
##  [79] "Franklin Richards"         "Galactus"                 
##  [81] "Gambit"                    "Gamora"                   
##  [83] "Garbage Man"               "Gary Bell"                
##  [85] "General Zod"               "Ghost Rider"              
##  [87] "Gladiator"                 "Godzilla"                 
##  [89] "Goku"                      "Gorilla Grodd"            
##  [91] "Greedo"                    "Groot"                    
##  [93] "Guy Gardner"               "Havok"                    
##  [95] "Hela"                      "Hellboy"                  
##  [97] "Hercules"                  "Hulk"                     
##  [99] "Human Torch"               "Husk"                     
## [101] "Hybrid"                    "Hyperion"                 
## [103] "Iceman"                    "Indigo"                   
## [105] "Ink"                       "Invisible Woman"          
## [107] "Jar Jar Binks"             "Jean Grey"                
## [109] "Jubilee"                   "Junkpile"                 
## [111] "K-2SO"                     "Killer Croc"              
## [113] "Kilowog"                   "King Kong"                
## [115] "King Shark"                "Krypto"                   
## [117] "Lady Deathstrike"          "Legion"                   
## [119] "Leonardo"                  "Living Tribunal"          
## [121] "Lobo"                      "Loki"                     
## [123] "Magneto"                   "Man of Miracles"          
## [125] "Mantis"                    "Martian Manhunter"        
## [127] "Master Chief"              "Medusa"                   
## [129] "Mera"                      "Metallo"                  
## [131] "Michelangelo"              "Mister Fantastic"         
## [133] "Mister Knife"              "Mister Mxyzptlk"          
## [135] "Mister Sinister"           "MODOK"                    
## [137] "Mogo"                      "Mr Immortal"              
## [139] "Mystique"                  "Namor"                    
## [141] "Nebula"                    "Negasonic Teenage Warhead"
## [143] "Nina Theroux"              "Nova"                     
## [145] "Odin"                      "One-Above-All"            
## [147] "Onslaught"                 "Parademon"                
## [149] "Phoenix"                   "Plantman"                 
## [151] "Polaris"                   "Power Girl"               
## [153] "Power Man"                 "Predator"                 
## [155] "Professor X"               "Psylocke"                 
## [157] "Q"                         "Quicksilver"              
## [159] "Rachel Pirzad"             "Raphael"                  
## [161] "Red Hulk"                  "Red Tornado"              
## [163] "Rhino"                     "Rocket Raccoon"           
## [165] "Sabretooth"                "Sauron"                   
## [167] "Scarlet Spider II"         "Scarlet Witch"            
## [169] "Sebastian Shaw"            "Sentry"                   
## [171] "Shadow Lass"               "Shadowcat"                
## [173] "She-Thing"                 "Sif"                      
## [175] "Silver Surfer"             "Sinestro"                 
## [177] "Siren"                     "Snake-Eyes"               
## [179] "Solomon Grundy"            "Spawn"                    
## [181] "Spectre"                   "Spider-Carnage"           
## [183] "Spock"                     "Spyke"                    
## [185] "Star-Lord"                 "Starfire"                 
## [187] "Static"                    "Steppenwolf"              
## [189] "Storm"                     "Sunspot"                  
## [191] "Superboy-Prime"            "Supergirl"                
## [193] "Superman"                  "Swamp Thing"              
## [195] "Swarm"                     "T-1000"                   
## [197] "T-800"                     "T-850"                    
## [199] "T-X"                       "Thanos"                   
## [201] "Thing"                     "Thor"                     
## [203] "Thor Girl"                 "Toad"                     
## [205] "Toxin"                     "Toxin"                    
## [207] "Trigon"                    "Triton"                   
## [209] "Ultron"                    "Utgard-Loki"              
## [211] "Vegeta"                    "Venom"                    
## [213] "Venom III"                 "Venompool"                
## [215] "Vision"                    "Warpath"                  
## [217] "Wolverine"                 "Wonder Girl"              
## [219] "Wonder Woman"              "X-23"                     
## [221] "Ymir"                      "Yoda"
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info %>% filter(alignment=="good")
```


```r
bad_guys <- superhero_info %>% filter(alignment=="bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
good_guys %>% tabyl(race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guys %>% filter(race=="Asgardian")
```

```
## # A tibble: 3 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif   Female blue      Asga… Black         188 Marvel C… <NA>       good     
## 2 Thor  Male   blue      Asga… Blond         198 Marvel C… <NA>       good     
## 3 Thor… Female blue      Asga… Blond         175 Marvel C… <NA>       good     
## # … with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guys %>% filter(race=="Human" & height>=200)
```

```
## # A tibble: 6 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
## 2 Bloo… Female blue      Human Brown         218 Marvel C… <NA>       bad      
## 3 Doct… Male   brown     Human Brown         201 Marvel C… <NA>       bad      
## 4 King… Male   blue      Human No Hair       201 Marvel C… <NA>       bad      
## 5 Liza… Male   red       Human No Hair       203 Marvel C… <NA>       bad      
## 6 Scor… Male   brown     Human Brown         211 Marvel C… <NA>       bad      
## # … with 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
good_guys %>% filter(hair_color=="No Hair")
```

```
## # A tibble: 37 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo… Male   yellow    Human No Hair       203 Marvel C… <NA>       good     
##  2 Abe … Male   blue      Icth… No Hair       191 Dark Hor… blue       good     
##  3 Abin… Male   blue      Unga… No Hair       185 DC Comics red        good     
##  4 Beta… Male   <NA>      <NA>  No Hair       201 Marvel C… <NA>       good     
##  5 Bish… Male   brown     Muta… No Hair       198 Marvel C… <NA>       good     
##  6 Blac… Male   brown     <NA>  No Hair       185 DC Comics <NA>       good     
##  7 Blaq… <NA>   black     <NA>  No Hair        NA Marvel C… <NA>       good     
##  8 Bloo… Male   black     Muta… No Hair        NA Marvel C… <NA>       good     
##  9 Crim… Male   brown     <NA>  No Hair       180 Marvel C… <NA>       good     
## 10 Dona… Male   green     Muta… No Hair        NA IDW Publ… green      good     
## # … with 27 more rows, and 1 more variable: weight <dbl>
```


```r
bad_guys %>% filter(hair_color=="No Hair")
```

```
## # A tibble: 35 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom… Male   green     Huma… No Hair     203   Marvel C… <NA>       bad      
##  2 Abso… Male   blue      Human No Hair     193   Marvel C… <NA>       bad      
##  3 Alien Male   <NA>      Xeno… No Hair     244   Dark Hor… black      bad      
##  4 Anni… Male   green     <NA>  No Hair     180   Marvel C… <NA>       bad      
##  5 Anti… Male   yellow    God … No Hair      61   DC Comics <NA>       bad      
##  6 Blac… Male   black     Human No Hair     188   DC Comics <NA>       bad      
##  7 Bloo… Male   white     <NA>  No Hair      30.5 Marvel C… <NA>       bad      
##  8 Brai… Male   green     Andr… No Hair     198   DC Comics green      bad      
##  9 Dark… Male   red       New … No Hair     267   DC Comics grey       bad      
## 10 Dart… Male   yellow    Cybo… No Hair     198   George L… <NA>       bad      
## # … with 25 more rows, and 1 more variable: weight <dbl>
```


```r
superhero_info %>% tabyl(hair_color, alignment)
```

```
##        hair_color bad good neutral NA_
##            Auburn   3   10       0   0
##             black   0    3       0   0
##             Black  42  108       8   0
##      Black / Blue   1    0       0   0
##             blond   1    2       0   0
##             Blond  11   85       1   2
##              Blue   1    1       1   0
##             Brown  27   55       4   0
##     Brown / Black   0    1       0   0
##     Brown / White   0    4       0   0
##            Brownn   1    0       0   0
##              Gold   1    0       0   0
##             Green   1    7       0   0
##              Grey   3    2       0   0
##            Indigo   0    1       0   0
##           Magenta   0    1       0   0
##           No Hair  35   37       3   0
##            Orange   0    2       0   0
##    Orange / White   0    1       0   0
##              Pink   0    1       0   0
##            Purple   3    1       1   0
##               Red   9   40       2   0
##        Red / Grey   1    0       0   0
##      Red / Orange   1    0       0   0
##       Red / White   0    1       0   0
##            Silver   0    3       0   1
##  Strawberry Blond   3    4       0   0
##             White  10   10       2   1
##            Yellow   0    2       0   0
##              <NA>  53  114       2   3
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 300 or weight over 450?

```r
superhero_info %>% 
  select(name, height, weight) %>% 
  filter(height>=300 | weight>=450) %>% 
  arrange(desc(height))
```

```
## # A tibble: 14 x 3
##    name          height weight
##    <chr>          <dbl>  <dbl>
##  1 Fin Fang Foom  975       18
##  2 Galactus       876       16
##  3 Groot          701        4
##  4 MODOK          366      338
##  5 Wolfsbane      366      473
##  6 Onslaught      305      405
##  7 Sasquatch      305      900
##  8 Ymir           305.      NA
##  9 Juggernaut     287      855
## 10 Darkseid       267      817
## 11 Hulk           244      630
## 12 Bloodaxe       218      495
## 13 Red Hulk       213      630
## 14 Giganta         62.5    630
```

11. Just to be clear on the `|` operator, lets have a look at the superheros over 300 in height...

```r
superhero_info %>% 
  select(name, height) %>% 
  filter(height>=300) %>% 
  arrange(desc(height))
```

```
## # A tibble: 8 x 2
##   name          height
##   <chr>          <dbl>
## 1 Fin Fang Foom   975 
## 2 Galactus        876 
## 3 Groot           701 
## 4 MODOK           366 
## 5 Wolfsbane       366 
## 6 Onslaught       305 
## 7 Sasquatch       305 
## 8 Ymir            305.
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>% 
  select(name, weight) %>% 
  filter(weight>=450) %>% 
  arrange(desc(weight))
```

```
## # A tibble: 8 x 2
##   name       weight
##   <chr>       <dbl>
## 1 Sasquatch     900
## 2 Juggernaut    855
## 3 Darkseid      817
## 4 Giganta       630
## 5 Hulk          630
## 6 Red Hulk      630
## 7 Bloodaxe      495
## 8 Wolfsbane     473
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the lowest height to weight ratio?

```r
superhero_info %>% 
  mutate(height_weight_ratio=height/weight) %>% 
  select(name, height_weight_ratio) %>% 
  arrange(height_weight_ratio)
```

```
## # A tibble: 734 x 2
##    name        height_weight_ratio
##    <chr>                     <dbl>
##  1 Giganta                  0.0992
##  2 Utgard-Loki              0.262 
##  3 Darkseid                 0.327 
##  4 Juggernaut               0.336 
##  5 Red Hulk                 0.338 
##  6 Sasquatch                0.339 
##  7 Hulk                     0.387 
##  8 Bloodaxe                 0.440 
##  9 Thanos                   0.454 
## 10 A-Bomb                   0.460 
## # … with 724 more rows
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names agility accelerated_hea… lantern_power_r… dimensional_awa…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, …
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>% 
  filter(accelerated_healing==TRUE & durability==TRUE & super_strength==TRUE)
```

```
## # A tibble: 97 x 168
##    hero_names agility accelerated_hea… lantern_power_r… dimensional_awa…
##    <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
##  1 A-Bomb     FALSE   TRUE             FALSE            FALSE           
##  2 Abe Sapien TRUE    TRUE             FALSE            FALSE           
##  3 Angel      TRUE    TRUE             FALSE            FALSE           
##  4 Anti-Moni… FALSE   TRUE             FALSE            TRUE            
##  5 Anti-Venom FALSE   TRUE             FALSE            FALSE           
##  6 Aquaman    TRUE    TRUE             FALSE            FALSE           
##  7 Arachne    TRUE    TRUE             FALSE            FALSE           
##  8 Archangel  TRUE    TRUE             FALSE            FALSE           
##  9 Ardina     TRUE    TRUE             FALSE            FALSE           
## 10 Ares       TRUE    TRUE             FALSE            FALSE           
## # … with 87 more rows, and 163 more variables: cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>,
## #   density_control <lgl>, stamina <lgl>, astral_travel <lgl>,
## #   audio_control <lgl>, dexterity <lgl>, omnitrix <lgl>, super_speed <lgl>,
## #   possession <lgl>, animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, …
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
kinesis <- superhero_powers %>% 
  select(hero_names, ends_with("kinesis"))
kinesis
```

```
## # A tibble: 667 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 3-D Man    FALSE       FALSE          FALSE       FALSE        FALSE       
##  2 A-Bomb     FALSE       FALSE          FALSE       FALSE        FALSE       
##  3 Abe Sapien FALSE       FALSE          FALSE       FALSE        FALSE       
##  4 Abin Sur   FALSE       FALSE          FALSE       FALSE        FALSE       
##  5 Abominati… FALSE       FALSE          FALSE       FALSE        FALSE       
##  6 Abraxas    FALSE       FALSE          FALSE       FALSE        FALSE       
##  7 Absorbing… FALSE       FALSE          FALSE       FALSE        FALSE       
##  8 Adam Monr… FALSE       FALSE          FALSE       FALSE        FALSE       
##  9 Adam Stra… FALSE       FALSE          FALSE       FALSE        FALSE       
## 10 Agent Bob  FALSE       FALSE          FALSE       FALSE        FALSE       
## # … with 657 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>% 
  filter(hero_names=="Wonder Woman") %>% 
  select_if(all_vars(.=="TRUE"))
```

```
## Warning: Using quosures is deprecated
## Please use a one-sided formula, a function, or a function name
## This warning is displayed once per session.
```

```
## # A tibble: 1 x 30
##   accelerated_hea… durability flight marksmanship weapons_master longevity
##   <lgl>            <lgl>      <lgl>  <lgl>        <lgl>          <lgl>    
## 1 TRUE             TRUE       TRUE   TRUE         TRUE           TRUE     
## # … with 24 more variables: intelligence <lgl>, super_strength <lgl>,
## #   telepathy <lgl>, stamina <lgl>, super_speed <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   enhanced_senses <lgl>, dimensional_travel <lgl>, enhanced_memory <lgl>,
## #   reflexes <lgl>, force_fields <lgl>, fire_resistance <lgl>,
## #   enhanced_hearing <lgl>, hypnokinesis <lgl>, enhanced_smell <lgl>,
## #   vision_telescopic <lgl>, toxin_and_disease_resistance <lgl>,
## #   magic_resistance <lgl>, vision_microscopic <lgl>, vision_night <lgl>,
## #   vision_infrared <lgl>, vision_x_ray <lgl>, vision_thermal <lgl>
```
