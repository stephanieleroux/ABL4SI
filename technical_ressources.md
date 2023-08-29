* From LB (2023-07-25):
> LB nemo conf manager: https://github.com/brodeau/nemo_conf_manager
> Forcing files and BDYs on Frazilo (/data/).

* From AA (2023-07-24):
> Mail from NEMO system team about the release of 4.2.1.

* From AA (2023-07-21):
> To build weights for interpo [https://github.com/auraoupa/grand-challenge-adastra-ORCA36/tree/main/eORCA05/BUILD/WEIGHT](https://github.com/auraoupa/grand-challenge-adastra-ORCA36/tree/main/eORCA05/BUILD/WEIGHT).

* From GS (2023-05-31) and Lemarié et al 2021: Steps of preprocessing to prepare the 3D atmos forcing files:
> 1. Téléchargement des forçage IFS sur les niveaux sigmas natifs (Température potentielle, Humidité, U, V).
> 2. On calcule les gradients de pression en premier le long des niveaux verticaux natifs  d’IFS avant de les projeter sur l’horizontale pour éviter les erreurs liées à l’interpolation verticale. L’interp verticale intervient donc après le calcul des gradients.
> 3.  Interpolation sur des niveaux d’altitude fixe (cf le fichier ABLdomain.cfg)
> 4. On applique un filtre de Shapiro pour lisser spatialement en enlevant  le bruit des petites échelles inférieures à 2*dx.  L’idée est de retirer les ondes de Gibbs (artefacts liés aux erreurs de troncatures lors du passage du domaine spectral vers le domaine physique) et les petites échelles liées à des processus non-géostrophiques. Il peut également y avoir des ruptures de la continuité de certaines variables près des côtes. Tout cela engendre localement des valeurs aberrantes lors du calcul des gradients qui peuvent ensuite polluer l’ABL et donc la réponse océanique.


* From GS (2023-05-31):
> Sur le ftp: il y a le domcfg de la grille verticale 50 niveaux de l'ABL et les différents fichiers de forçage 3D.
Même si l'année ne correspond pas à votre période d'étude, ça fera le job pour faire juste des tests.
Attention, la température est déjà convertie en température potentielle, il ne faut donc pas la reconvertir une nouvelle fois dans Nemo.
Il y a un flag "ln_tpot" qui sert à cela en namelist.
Et il faudra aussi que tu génères de nouveaux poids pour l'interp online, et ça devrait être bon.

> Par contre, la résolution d'ERAI étant particulièrement faible au pôle nord à cause de la grille gaussienne réduite d'IFS, les gradients de pression vont être particulièrement bruités/moches. C'est ce point là qu'il faut surveiller avec attention et que je voudrais essayer d'améliorer, mais je n'ai pas encore trouver la bonne méthode et cela va nécessité un peu plus de boulot (c'est aussi vrai avec ERA5 dans une moindre mesure).

> Pour lancer la simu ABL, voilà les tests nemo que tu pourrais faire avant: 1. lancer la simu en mode bulk mais en utilisant les forçages ABL (Nemo ne prendra que le 1er niveau vertical des fichiers): ça te donnera une simu de rèf, 2. lancer une simu en mode ABL avec le nudging à 100%: tu devrais retomber sur les mêmes résultats que la simu bulk, 3. enfin dans une 3ème simu, relâcher la relaxation et croiser les doigts :)

* From SLX (2023-04-28):
> Zenodo archive for the Lemarié et al paper: [https://zenodo.org/record/3904518#.ZEu8UHZBwa4](https://zenodo.org/record/3904518#.ZEu8UHZBwa4).

* From SLX (2023-04-06):
> My [step-by-step technical notes on running ORCA2_ICE_ABL reference config (NEMO4.2)](https://github.com/stephanieleroux/ABL4SI/blob/main/DOCS/ORCA2-ABL1D_NEMO-REF-CONFIG_technotes.md) 

* From AA (2023-04-05):
> Tutorial to run a NEMO reference config: https://zenodo.org/record/6817000#.ZC0pLvc69WJ

* From AA (2023-02-21):
> NEMO test case with ABL : not yet described in the [documentation](https://sites.nemo-ocean.io/user-guide/cfgs.html) but inputs and forcings are available on [SETTE website](https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/)

* From FL (2023-02-14):
> Pour commencer il faudrait partir sur une config pas trop couteuse pour mettre les choses en place (par exemple 1/4° avec le module de surface en standalone, i.e. en spécifiant la SST).
Dans nemo 4.2 il y a une config de référence ORCA2_ICE_ABL. Il devrait y avoir une doc ABL dans la doc de référence NEMO et les outils de preprocessing (ABL_TOOLS) sont inclus dans le répertoire TOOLS de nemo 4.2  (il s'avère qu'il faudra mieux passer par Guillaume et moi pour le preprocessing car on pourrait imaginer te fournir des fichiers déjà processés si on part sur ERA5 + interpolation online). 
 
