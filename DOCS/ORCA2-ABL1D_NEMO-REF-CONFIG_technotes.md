# ORCA2_ICE_ABL NEMO reference config on JeanZay@IDRIS
My step-by-step technical notes on running the ORCA2-ABL1D NEMO reference config on JeanZay@IDRIS.

_Last update: 2023-04-21_

## Install xios

```
cd $WORK/DEV

svn co http://forge.ipsl.jussieu.fr/ioserver/svn/XIOS/trunk@2430 xios-2.5@2430
cd xios-2.5@2430
./make_xios --arch X64_JEANZAY --full --prod --job 8 
```

## Get NEMO4.2 and compile
* Get release 4.2.0:

```
cd /gpfswork/rech/cli/regi915/CONFIGS/CONFIG_ORCA2_ICE/

# get branch main
git clone  https://forge.nemo-ocean.eu/nemo/nemo.git ORCA2_ABL-JZSLX.02
```

* Edit arch file:

```
cp arch/CNRS/arch-X64_JEANZAY.fcm arch/CNRS/arch-X64_JEANZAY_slx.fcm  

vi arch/CNRS/arch-X64_JEANZAY_slx.fcm 

# edit: %XIOS_HOME            /gpfswork/rech/cli/rote001/DEV/xios_trunk_2430
# Note: does nt work yet /gpfswork/rech/cli/regi915/DEV/xios-2.5@2430 so i use xios from AA.
```

* Compile:

```
./makenemo -m X64_JEANZAY_slx -r ORCA2_ICE_PISCES -n ORCA2_ABL-JZSLX.02 -j 8
```
_Note: building here an executable from the reference config ORCA2_ICE_PISCES which includes all these sub-components:  OCE TOP ICE NST ABL. Should not use the reference config ORCA2_ICE_ABL as it is not an official ref config._

## Prepare first run:
* get input files:

```
cd /gpfswork/rech/cli/regi915/ORCA2/inputfiles

wget “https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/r4.2.0/ORCA2_ABL_v4.2.0.tar.gz”

wget "https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/r4.2.0/ORCA2_ICE_v4.2.0.tar.gz"
```


* Prepare run directory:

```
cd ./cfgs/ORCA2_ABL-JZSLX.02/

cp -r EXP00/ EXPREF
cd EXP00
```

* Edit namelists to mimic sette script `/sette/sette_reference-configurations.sh` for ORCA2_ICE_PISCES config:
 
---

_latest status (2023-04-21):_
Success! ORCA2_ICE_PISCES  ran for 2 months with ABL switched on.

_Older comment (2023-04-06):_
GS said ORCA2_ICE_ABL is not an official reference config. Should use ORCA2_ICE_PISCES instead (which permits to activate ABL).

_Older comment (2023-04-06):_
Input files from `ORCA2_ABL_v4.2.0.tar.gz` do not seem to match with files needed by the namelist shared with the reference config `ORCA2_ICE_ABL`. Question asked to FL and GS.
