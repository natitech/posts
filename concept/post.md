# Un mot sur la conceptualisation

*Pourquoi, quand et comment être efficace lors du nommage*

Le nommage est souvent présenté comme l'une des choses les plus difficiles du développement logiciel.

Le client - ou celui qui exprime le besoin - communique dans sa langue une demande d'automatisation. Une partie de son vocabulaire est utilisé dans le code. C'est le fameux "langage ubiquiste" [^10]. 

Ce vocabulaire est souvent incomplet. D'une part, le client n'a pas toujours conscience de la complexité qui se cache derrière sa demande et de la nécessité d'**introduire de nouveaux concepts**. D'autre part, il faudra **créer des abstractions dans le code** et donc proposer de nouveaux termes.

En général, cette activité est plaisante. Il y a en effet quelque chose de très créatif à révéler une idée qui était cachée. Mais c'est aussi une activité assez intense, avec parfois un cumul de plusieurs heures passées à tâtonner. 

Se posent alors les questions suivantes : ce temps passé est-il vraiment nécessaire dans tous les cas ? Est ce qu'il est possible d'optimiser cette recherche et de trouver efficacement le bon terme ?

### Lisibilité et enjeux

Mais avant, quelques rappels.

Une très grande partie du temps de développement est consacré à lire du code [^20] [^30]. Les 2 tiers du code sont constitués par des termes [^40]. Autrement dit, une grande majorité du temps est passée à lire des termes. 

Trouver les bons termes est donc faire preuve d'empathie pour tous les lecteurs du code. C'est même ainsi qu'est rendu possible le travail en équipe. 

Même en étant seul, quelques années (ou quelques mois...) suffisent à devenir étranger à son propre code. Or, la maintenance d'un projet représente généralement 70% du cout total du projet [^50]. 

Certaines études essayent de creuser d'avantage cette notion de lisibilité en l'associant à la notion de charge cognitive. Il devient alors possible de quantifier une amélioration ou une détérioration de lisibilité en mesurant l'activité cérébrale ou la précision et le temps de réponse à des tests de compréhension de code [^60] [^61]. 

Quelles que soient les méthodes utilisées, il parait imprudent de ne pas tenir compte de la lisibilité et du nommage tant les études convergent [^70].

### Homogénéité et syntaxe

Pour améliorer le nommage, il existe de nombreuses recommandations de syntaxe. On y trouve par exemple des arbitrages entre camelCase et snake_case. 

Sur ce type de recommandation, très souvent, chacun a ses habitudes et les arguments ne sont pas décisifs. Théoriquement, les neurosciences peuvent aider à trancher des débats souvent stériles. Mais en pratique, c'est moins clair. Par exemple, une étude basée sur l'enregistrement du mouvement des yeux tend à conclure que la notation snake_case demande moins d'effort en lecture mais est plus imprécise que le camelCase [^80].

Un autre exemple intéressant de convention est l'idée de "moule", c'est-à-dire la manière dont les termes vont se combiner pour constituer un nom. Par exemple, la convention dite "big-endian" où une variable est suffixée par ordre d'importance [^90]. Le nombre maximum de commandes par mois serait donc *orders_per_month_max* plutôt que *max_orders_per_month*.

Globalement, dans toutes ces recommandations, **le plus important est l'homogénéité**. Si, pour certains, le camelCase est plus lisible que le snake_case, il est en revanche certain pour tout le monde que le mélange des deux sera plus dur à lire.

L'équipe doit donc se mettre d'accord sur un jeu de conventions qui sera appliqué dans un projet (et même des moyens d'en assurer le respect). Tout le monde gagnera en lecture et un nouveau contributeur aura plus de facilité à adopter ces conventions. La syntaxe ne devrait donc pas demander beaucoup d'effort.

### Homogénéité et langage

Dans le prolongement de l'idée d'homogénéité, il peut être **préférable d'utiliser des termes quasi-exclusivement anglais**, y compris pour un domaine métier qui serait dans une autre langue :
- Le travail de compréhension du code est déjà assez dur, inutile donc de mélanger deux langues
- Un nouveau contributeur ne saura pas non plus quand utiliser une langue ou l'autre
- Aucune étude comparative ne semble pouvoir l'affirmer, mais l'anglais est parfois considéré plus efficace pour la conceptualisation car plus concis

```
//En franglais                                 //En anglais
if colis.isTropVolumineux(modeDeTransport)     if package.isTooLarge(shipping)
    throw new TransportException()                 throw new ShippingException()
```

### Niveau d'abstraction et renommage

Après tous ces rappels, il faut d'abord examiner les situations où le temps passé à chercher un terme est vraiment nécessaire.

Et il s'avère que **le temps investi devrait être proportionnel au niveau d'abstraction**. 

En effet, les caractéristiques d'un code de bas niveau d'abstraction font que l'impact d'un mauvais nommage est limité :
- Il est précisémment moins abstrait : donc souvent plus facile à nommer
- Il a une portée plus limitée (ou un couplage afférent plus bas) : donc plus facile à *re*nommer
- Il est plus rarement lu car, très souvent, le lecteur du code ne rentre pas dans tous les détails d'implémentation et reste le plus souvent assez haut dans l'abstraction avant de creuser : donc le nommage aura moins d'impact sur la lisibilité globale du projet
- Dans le prolongement de l'idée précédente, tout ce qui est lu avant d'arriver au code de bas niveau va aider à contextualiser : donc le nommage est moins important

![Langages et niveau d'abstraction](img/languages.svg)

Au contraire, l'effort de nommage doit se concentrer sur le haut niveau d'abstraction car :
- Beaucoup d'autres termes seront forgés à partir de là
- Il peut être plus difficile de renommer tout un module que de renommer une variable dans une fonction de quelques lignes
- Il est possible que le terme soit utilisé dans une interface graphique et donc dans des traductions, ce qui augmente encore la difficulté de renommage
- Si le terme se retrouve dans le langage ubiquiste [^10], il peut y avoir une certaine inertie d'usage par l'équipe ou le client

### Qu'est-ce qu'un terme ?

A ce stade, il devient nécessaire de repréciser cette notion de "terme".

Une recommandation assez classique [^20] est d'utiliser des verbes pour les méthodes, et des noms communs pour les classes. Comme le terme doit désigner un concept, l'idéal serait :
- Qu'il ne soit pas composé de plusieurs mots car il y a de fortes chances que nous soyons obligés de l'associer à plusieurs autres termes (techniques ou non)
- Qu'il puisse se décliner en nom commun ou en verbe selon l'usage qui en sera fait
- Qu'il puisse se décliner facilement au pluriel 

En anglais, il est a priori plus simple de transformer un nom commun en verbe que l'inverse. C'est pour cette raison que la recherche d'un terme devrait plutôt être une recherche d'un nom commun.

Quelques exemples de déclinaisons nom -> verbe :
- *Shipping* -> ship() : assez naturel
- *Discount* -> discount() : impossible de distinguer le nom du verbe
- *Digital* -> digitalize() ou digitize() : risque de confusion alors que ne portent pas le même sens
- *Unicorn* -> unicornize() : le verbe est compréhensible mais il n'existe pas en anglais, pas très élégant

### Lister des candidats

Avant tout, l'écriture de tests et de code nécessite beaucoup de ressources mentales. Le travail de conceptualisation devrait donc être une tâche à part, parfois en collaboration directe avec le client.

Il existe des modèles de nommage, notamment celui de Feitelson [^100] vulgarisé par Felienne Hermans dans son livre [^110]. Pour faire simple, il s'agit de sélectionner les concepts, de trouver les termes et de les associer (en suivant un moule). Voici ce qu'elle décrit sur le choix des termes:

> Often choosing the right words is straightforward, with one specific word being the obvious choice because it is used in the domain of the code or has been used across the codebase. However, in his experiments Feitelson observed that there were also **many cases in which for at least one of the words many different contending options were suggested by participants**. Such diversity can cause problems when developers become confused about whether synonyms mean the same thing or represent nuanced differences.

Pour trouver les candidats, il est tout simplement possible d'**utiliser un dictionnaire de synonymes** sur une ou plusieurs propositions initiales de noms communs. Si la langue du métier n'est pas l'anglais, il est aussi possible d'obtenir d'**autres suggestions par traduction et même par rétrotraduction**. 

![Process de recherche d'un terme](img/process.svg)

Par exemple :
- La description faite par le client : *contenu d'un camion de livraison (sable, fut d'huile, ou métaux à recycler par exemple)*
- Le client lui-même n'a pas de terme qui regroupe tout, il passe systématiquement par la désignation réelle, mais il devient nécessaire de créer ce concept dans le code
- Une suggestion initiale est *chargement*
- Quelques synonymes : *affrètement*, *charge*
- Les traductions donnent : *loading*, *freight*, *chartering*, *cargo*, *shipment*, *payload*

Globalement, il est recommandé d'éliminer les termes trop équivoques, par exemple ne pas utiliser *Manager* ou *Data* ("weasel words" en anglais) [^140]. Il est aussi préférable d'éviter les patrons de développement (comme *Factory*) ou les termes d'infrastructure (comme *Container*).

### Évaluer le meilleur terme 

Natif anglais ou non, il faudra ensuite se faire une idée précise des **nuances de sens**. Comme lu récemment dans une série de conseils à destination des écrivains : 

> Finding the word that packs the most punch requires both a great vocabulary and a great understanding of the nuances in English. [^111]

En effet, les différences de sens sont parfois subtiles : par exemple, quelles différences entre *Voucher*, *Coupon*, *Discount* ? 

Comme autre critère que le sens, il est possible de reprendre les **critères linguistiques** déjà énoncés et de les compléter. Il faut que le terme :
- Puisse se décliner en verbe (pour en faire une méthode) 
- Puisse se décliner au pluriel de manière claire. Vu dans une base de code, *Quantum* et *Quanta* sont utilisés pour désigner des ensembles de quantité. Mais avec parfois *Quantums*, *Quantas* et parfois *Quanti* ou même *Quantis* pour finir en *Quantits*...
- Comporte un nombre de lettres qui soit un compromis entre vitesse de compréhension [^120] et mémoire à court terme [^130], donc plus de 3 lettres (en fait, au moins une syllabe, ce qui rejoint aussi une recommandation que le terme soit prononçable) et moins de 15 lettres. Cette dernière limite est assez arbitraire mais il n'y a de toute façon plus beaucoup de mots à partir de 15 lettres [^131]. D'autre part, même avec 14 lettres, *Transportation* peut devenir par exemple *TransportationStrategyImplementation*, ce qui fait déjà assez long.
- Soit dans le registre courant (par opposition à familier et soutenu ou même littéraire) : comme ordre de grandeur, il y a envion 600.000 mots dans le dictionnaire d'Oxford, 300.000 qui ne sont pas considérés obsolètes, 30.000 qui sont utilisés couramment. Il est assez dommage de se priver de 270.000 mots mais est ce que *Peregrination* peut être préférable à *Travel* ?

Pour aller encore un peu plus loin dans cette approche purement linguistique, il existe un concept intéressant appelé "hyperonymie" [^150]. Il s'agit tout simplement du terme linguistique pour désigner l'abstraction ! Un *animal* est l'hyperonyme d'un *lion*. A l'inverse, une *gazelle* est l'hyponyme d'un *animal*. L'ensemble des hyperonymes forme une taxonomie.

Ainsi, un **bon terme devrait être aussi un hyperonyme**, mais pas trop haut dans la taxonomie pour ne pas devenir trop générique non plus.

Par exemple, à partir de la taxonomie suivante : *Mobility* > *Transportation* > *Vehicle* > *Automobile* > *Car* > *Sedan*. Est ce que *Mobility* ne serait pas systématiquement trop abstrait ? Sur cet exemple, à partir de *Car* et pour la suite de la taxonomie, il ne s'agit plus tellement de concepts ou d'abstractions dificilles à trouver. Et si le métier ne parle que de *voiture*, le terme devrait être *Car* et non *Vehicle* ([YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)). 

### Possibilité d'un glossaire neutre

Lister tous ces critères permet de faire le constat que **tous les mots du dictionnaire anglais ne sont pas de bons candidats**. 

Il devrait donc être possible de construire un grand glossaire qui soit neutre d'un point de vue du domaine métier et qui listerait les termes acceptables d'un point de vue linguistique. Il n'y aurait plus qu'à piocher dans ce glossaire de quelques milliers de mots (?).

Cette idée est peut-être farfelue ou irréalisable ; il semble en tout cas que personne n'ait proposé la création d'un tel glossaire dans la littérature ou dans un projet ouvert. 

Mais même s'il était possible de le construire, le problème est de **trouver un mot à partir d'un autre, ou à partir d'une définition**. Avoir la liste ne suffit donc pas à être efficace, il faudrait pouvoir aussi construire un dictionnaire de synonymes ou même un dictionnaire inversé.

Si seulement il existait un outil qui permette de saisir une description et d'obtenir une liste de propositions de mots... 

### Pertinence des LLM

Et oui, **un [LLM (Large Language Model)](https://en.wikipedia.org/wiki/Large_language_model) semble être l'outil idéal** :
- L'interaction avec un LLM est généralement sous la forme d'un agent conversationnel ou d'un prompt, ce qui correspond tout à fait au besoin. 
- Ces modèles sont intrinsèquement capables de comprendre les critères linguistiques et les liens entre les mots. 

Il suffit donc théoriquement de saisir 1/ une description, 2/ les critères d'évaluation, et 3/ de demander une liste de propositions. 

En partant du même exemple de description faite par le client qu'énoncé plus haut : *contenu d'un camion de livraison (sable, fut d'huile, ou métaux à recycler par exemple)*, voici un exemple sans doute un peu naïf de prompt avec ces 3 paragraphes :

```
Je recherche un terme en anglais qui désigne le contenu d'un camion de livraison (sable, fut d'huile, ou métaux à recycler par exemple). 

Ce terme doit avoir les caractéristiques suivantes : un seul mot, nom commun, hyperonyme, de plus de 3 lettres, de moins de 15 lettres, du registre courant, qui ne soit pas considéré comme ambigu. 

Fais 5 propositions distinctes de termes triées par usage dans la langue anglaise avec pour chacune le terme au singulier, au pluriel et quelques verbes anglais associés.
```

La réponse obtenue sur ChatGPT (v3.5, octobre 2023) est la suivante :

```
Cargo / Cargos / Load, Unload, Transport, Deliver
Goods / Goods / Transport, Ship, Deliver, Haul
Freight / Freights / Ship, Transport, Haul, Deliver
Load / Loads / Load, Unload, Transport, Carry
Shipment / Shipments / Ship, Receive, Transport, Deliver
```

A noter que le premier paragraphe de description joue beaucoup dans les propositions faites. A la question subsidiaire :
```
Pourquoi pas "Payload" ?
````

La réponse du LLM est :

```
"Payload" est un terme couramment utilisé pour désigner plutôt la charge utile transportée par un véhicule.
```

Il y a surement la possibilité d'améliorer le prompt car les résultats restent imparfaits :
- *Goods* : identique au pluriel et au singulier et beaucoup trop général
- *Load* : pas de différence avec le verbe et un peu trop connoté
- *Freight* : plutôt dans le domaine aérien ?
- *Shipment* comporte une intention d'expédition qui n'est pas pertinente ici
- *Payload* : pourrait être un bon candidat même si, en effet, souvent utilisé différemment 
- Reste *Cargo*

A noter qu'il doit aussi être possible de forcer le LLM à s'appuyer plutôt sur des données d'entrainement de type "codes sources". Le même prompt en remplaçant les critères linguistiques par des critères de bonne pratique de développement logiciel ("Ce terme doit respecter les bonnes pratiques de nommage dans le développement logiciel") semblent paradoxalement donner des résultats un peu moins bons.

Tout ceci reste à explorer...

### Conclusion

Finalement, le nommage reste difficile !

Pouvoir modifier le code sereinement permet d'atténuer les effets d'un mauvais nommage. Mais l'introduction de certains concepts doivent quand même faire l'objet d'une vraie réflexion en amont. 

Il existe quelques critères notamment linguistiques qui permettent d'être plus efficace à ce moment-là. Il est aussi possible que la solution soit un bon usage des LLM.

A ce sujet. Comme dans beaucoup d'autres métiers, l'émergence des LLM est une invitation à l'introspection, à la réflexion en profondeur du rôle d'un ingénieur logiciel :
- Certains proposent que les tests restent la prérogative des humains et que la machine s'occupe d'écrire le code de production ;
- D'autres soulignent que les LLM sont incapables d'écrire du code propre notamment parce que les LLM sont incapables de faire du Test Driven Development. De l'aveu même de la machine (!), "Writing tests is typically a task performed by human developers to ensure the correctness and robustness of their software." [^160]

Personnellement, il y a encore quelques temps, je pensais que notre véritable rôle pourrait justement être de rendre lisible la complexité par le nommage et que notre imagination produirait de meilleurs résultats. Mais je n'en suis désormais plus si sûr... A moins d'écrire un logiciel extrêmement original, la plupart du temps nous cherchons à trouver des termes dont l'usage n'est pas confidentiel. Il parait donc légitime de demander à un LLM. Il peut être regrettable d'utiliser une langue ainsi appauvrie et consensuelle mais il faut probablement l'assumer dans ce cas.

**Finalement, qu'en pensez-vous ?** Le nommage doit-il être aussi compliqué ? Y a-t-il d'autres outils / process ? Faut-il utiliser les LLM pour cela ? Quels sont les meilleurs prompts ?

---

*Nommage, Concept, Abstraction, Ubiquitous language, Efficacité, Linguistique, LLM*

© 2023 - Antoine ROLLAND - licensed under [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1)

[^10]: Eric Evans. Domain Driven Design: Tackling Complexity in the Heart of Software. 2003

[^20]: R. C. Martin. Clean Code, A Handbook of Agile Software Craftsmanship. 2011

[^30]: [Klaus Bayrhammer. You spend much more time reading code than writing code. 2020](https://bayrhammer-klaus.medium.com/you-spend-much-more-time-reading-code-than-writing-code-bc953376fe19)

[^40]: [F. Deißenböck and M. Pizka. Concise and consistent naming. 2005](https://itestra.com/wp-content/uploads/2018/03/06_itestra_concise_and_consistent_naming.pdf)

[^50]: B. Boehm and V. R. Basili. Software defect reduction top 10 list. 2001

[^60]: [Sarah Fakhoury, Yuzhan Ma, Venera Arnaoudova, and Olusola Adesope. The Effect of Poor Source Code Lexicon and Readability on Developers' Cognitive Load. 2018](https://doi.org/10.1145/3196321.3196347)

[^61]: [J. Siegmund, C. Kästner, S. Apel, C. Parnin, A. Bethmann, T. Leich, G. Saake, and A. Brechmann. Understanding source code with functional magnetic resonance imaging. 2014](https://kilthub.cmu.edu/articles/Understanding_Understanding_Source_Code_with_Functional_Magnetic_Resonance_Imaging/6626357/files/12123905.pdf)

[^70]: [Delano Oliveira, Reydne Bruno, Fernanda Madeiral†, Fernando Castor. Evaluating Code Readability and Legibility: An Examination of Human-centric Studies. 2021](https://arxiv.org/pdf/2110.00785.pdf)

[^80]: [Bonita Sharif and Jonathan I. Maletic. An Eye Tracking Study on camelCase and under_score Identifier Styles. 2010](https://www.researchgate.net/profile/Bonita-Sharif/publication/224159770_An_Eye_Tracking_Study_on_camelCase_and_under_score_Identifier_Styles/links/00b49534cc03bab22b000000/An-Eye-Tracking-Study-on-camelCase-and-under-score-Identifier-Styles.pdf)

[^90]: [HackerNews discussion around naming a very specific variable.](https://news.ycombinator.com/item?id=31436814)

[^100]: [Dror G. Feitelson Ayelet Mizrahi Nofar Noy Aviad Ben Shabat Or Eliyahu Roy Sheffer. How Developers Choose Names. 2021.](https://arxiv.org/pdf/2103.07487.pdf)

[^110]: Felienne Hermans. The Programmer's Brain. 2021

[^111]: [Sean Glatch. The importance of word choice in writing. 2022](https://writers.com/word-choice-in-writing)

[^120]: [J. C. Hofmeister, J. Siegmund, and D. V. Holt. Shorter Identifier Names Take Longer to Comprehend. 2019](https://www.infosun.fim.uni-passau.de/publications/docs/HoSeHo17.pdf)

[^130]: D. Binkley, D. Lawrie, S. Maex, and C. Morrell. Identifier length and limited programmer memory. 2009

[^131]: [Reginal Smith. Distinct word length frequencies: distributions and symbol entropies. 2012](https://arxiv.org/pdf/1207.2334.pdf)

[^140]: [Vladimir Khorikov. Ubiquitous Language and Naming. 2017](https://enterprisecraftsmanship.com/posts/ubiquitous-language-naming/)

[^150]: [Source Code Analysis and Natural Language Laboratory. Identifier Naming Structure Catalogue. 2022](https://github.com/SCANL/identifier_name_structure_catalogue#linguistic-antipatterns)

[^160]: [Jake Ogden. Job market risk assessment: clean code vs. AI code. 2023](https://cleancoders.com/blog/2023-10-29-Job-Market-Risk-Assessment-Clean-Code-vs-AI-Code)
