* From FL (2023-02-14):
> Pour commencer il faudrait partir sur une config pas trop couteuse pour mettre les choses en place (par exemple 1/4° avec le module de surface en standalone, i.e. en spécifiant la SST).
Dans nemo 4.2 il y a une config de référence ORCA2_ICE_ABL. Il devrait y avoir une doc ABL dans la doc de référence NEMO et les outils de preprocessing (ABL_TOOLS) sont inclus dans le répertoire TOOLS de nemo 4.2  (il s'avère qu'il faudra mieux passer par Guillaume et moi pour le preprocessing car on pourrait imaginer te fournir des fichiers déjà processés si on part sur ERA5 + interpolation online). 
 
* From AA (2023-02-21):
> NEMO test case with ABL : not yet described in the [documentation](https://sites.nemo-ocean.io/user-guide/cfgs.html) but inputs and forcings are available on [SETTE website](https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/)
