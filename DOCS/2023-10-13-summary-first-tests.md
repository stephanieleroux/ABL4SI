_Date: 2023-10-13_

## Configuration NEMO préparée:

* Config régionale Arctique (NANUK4) au 1/4° en version 4.2.1.
* Forçage atmosphérique:  ERAi (fichier 3d) pour 2013 fourni par Guillaume.
ABL ou BLK.
* Glace: SI3 avec rhéologie standard (aEVP) ou rhéologie cassante BBM implémentée(*) récemment par Laurent.



## A. “Sanity checks” sur les premières simulations réalisées:
Mes premiers tests sont tous avec la rhéologie standard de SI3 pour l’instant (aEVP). Et je n’ai fait tourner que 30 jours de simulation.

1. En suivant les conseils de Guillaume j’ai fait un premier  test pour vérifier qu’une simulation ABL+nudging très fort (temps de rappel = pas de temps) donnait une solution très proche d’une simulation de référence en Bulk standard avec le même fichier de forçage ERAi-2013.  
→ _Semble OK (solutions quasi similaires dans les deux cas)._
→ _[TO-DO] Re-vérifier les fichiers des poids pour l’interpolation on-the-fly du forçage atmosphérique (quelques motifs bizarres dans la solution au niveau du pôle nord)._


2. Ensuite j’ai testé une  simulation ABL avec nudging tel que conseillé dans le  papier Lemarié et al  2021 (temps de rappel du nudging en bas de l’ABL =  5 fois le pas de temps du modèle, qui est de  12 min = 720 s dans cette config), et le guidage du vent par le gradient de pression fourni par Guillaume. La solution de cette simulation avec ABL semble “raisonnable”, elle diffère un peu de la solution BLK sans être trop différente, et je vois que le vent turbulent semble se développer dans l’ABL, mais les différences de vent entre les simulations BLK et ABL restent relativement petites comparées au vent géostrophique. 

→ _Cette simu semble OK au premier ordre. _

→ _Comment faire un “sanity check” simple sur cette simu  pour vérifier que l’ABL est bien réglée, que je n’ai pas fait de grosse erreur dans la namelist, etc, avant de me lancer dans des analyses plus poussées de l’effet sur la glace?
La seule idée qui me vient à l’esprit c’est de vérifier qu’on retrouve bien le  “thermal feedback” de  l’océan comme montré dans le papier de 2021. Mais pour ça, il me faut déjà des simulations de quelques années, puis filtrer la mésoéchelle et calculer la correlation entre anomalie de SST et anomalie de U10 ou du stress sur l’océan. C’est déjà un diagnostic élaboré, c’est un peu dommage de mettre tout ça en oeuvre juste pour vérifier que la config tient la route. Et puis comme je suis sur l’Artique au 1/4°, je n’ai que la partie Nord du Gulf Stream  dans le sud de mon domaine (qui descend à 40°N) qui sera à peu près eddy resolving et turbulente, et où on est sûr que le thermal feedback devrait se voir._

—> _Y aurait-il une autre manière de vérifier que l’ABL tient la route avant de me lancer dans les simus longues?_


## B. Vent relatif vs vent absolu dans le calcul du stress…
A terme, l’idée est de tester l’effet de l’ABL dans une simulation où l’on utilisera la rhéologie de glace “cassante (“BBM”) que Laurent a implémentée récemment dans SI3. Mais je me suis rendue compte que quelques ajustements du code sont nécessaires pour que les motifs de Laurent soient pris en compte quand on active l’ABL, car certains de ses motifs sont dans la partie `if ln_blk` de `sbcblk.F90`. Je suis donc en train d’y bosser ce jours ci, et j’ai du mettre le nez dans le détail du code pour cela. Du coup  il y a qqchose qui me chiffonne à propos de la manière dont est calculé le stress:
Dans la namelist de l’ABL il y a le paramètre `rn_vfrac` qui permet de calculer le stress  en vent absolu ( `rn_vfrac=0`) ou en vent relatif (`rn_vfrac=1`).

Mais, pour le calcul du stress dans `ablmod.F90` on utilise aussi  `pcd_du`  (input parameter), et celui ci est calculé comme Cd x `wndm`. Et, sauf erreur de ma part,  il me semble que `wndm`est simplement calculé comme le modulel du vent lu dans le fichier atmosphérique, et que la vitesse de la glace ou de l’océan ne lui ai jamais soustraite.
Donc si on choisit `rn_vfrac=1`, le stress calculé dans ablmod.F90 est calculé comme:
Tau = rho_air * Cd  * Urel * |Ua|
Ua étant juste le vent lu du forçage atmosphérique, et Urel étant (Ua - Ice) ou (Ua - Uoce).

—> _Est-une approximation volontaire? Ou bien est-ce une approximation non-volontaire venant des changements en NEMO 4.2 ? J’ai creusé  un peu j’ai l’impression que l’option “vent relatif” pour les bulks air-sea n’est maintenant dispo qu’au travers de la param de Lionel Renauld avec le switch `ln_crt_fbk` dans la namelist pour contrer l’effet d’eddy killing. Mais ça n’a pas de sens d’utiliser cette  param lorsqu’on allume l’ABL, me semble -t-il, si?
Qu’en pensez-vous? La contribution de la vitesse de l’océan ou de la glace est très petite par rapport au vent (et à l’incertitude sur le vent des réanalyses)  mais ne perd-on pas un peu de l’intérêt d’utiliser une ABL interactive si on ne calcule pas  le stress en vent relatif?_



