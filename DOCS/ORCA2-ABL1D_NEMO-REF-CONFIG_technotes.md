# ORCA2_ICE_ABL NEMO reference config on JeanZay@IDRIS
My step-by-step technical notes on running the ORCA2-ABL1D NEMO reference config on JeanZay@IDRIS.

**Log:**

* _Latest status (2023-07-24):_
ORCA2_ICE_PISCES set to new release 4.2.1. Success with reference case.

* _Older comment (2023-04-21):_
Success! ORCA2_ICE_PISCES  ran for 2 months with ABL switched on.

* _Older comment (2023-04-06):_
GS said ORCA2_ICE_ABL is not an official reference config. Should use ORCA2_ICE_PISCES instead (which permits to activate ABL).

* _Older comment (2023-04-06):_
Input files from `ORCA2_ABL_v4.2.0.tar.gz` do not seem to match with files needed by the namelist shared with the reference config `ORCA2_ICE_ABL`. Question asked to FL and GS.

---

## Install xios

```
cd $WORK/DEV

svn co http://forge.ipsl.jussieu.fr/ioserver/svn/XIOS/trunk@2430 xios-2.5@2430
cd xios-2.5@2430
./make_xios --arch X64_JEANZAY --full --prod --job 8 
```

## Get NEMO4.2 and compile
* Get release 4.2.1:

```
cd /gpfswork/rech/cli/regi915/CONFIGS/CONFIG_ORCA2_ICE/

git clone  https://forge.nemo-ocean.eu/nemo/nemo.git nemo_4.2.1_ORCA2_ICE-JZSLX.10
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
./makenemo -m X64_JEANZAY_slx -r ORCA2_ICE_PISCES del_key key_top -n ORCA2_ICE-JZSLX.10 -j 8
```
_Note: building here an executable from the reference config ORCA2_ICE_PISCES which includes all these sub-components:  OCE TOP ICE NST ABL. Should not use the reference config ORCA2_ICE_ABL as it is not an official ref config. The above line remove TOP (PISCES) from the compilation as we will not use it._

## Prepare first run:
* get input files:

```
cd /gpfswork/rech/cli/regi915/ORCA2/inputfiles

wget “https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/r4.2.0/ORCA2_ABL_v4.2.0.tar.gz”

wget "https://gws-access.jasmin.ac.uk/public/nemo/sette_inputs/r4.2.0/ORCA2_ICE_v4.2.0.tar.gz"
```


* Prepare run directory:

```
cd ./cfgs/ORCA2_ABL-JZSLX.10/

cp -r EXP00/ EXPREF
cd EXP00
```

* Edit namelists to mimic sette script `/sette/sette_reference-configurations.sh` for ORCA2_ICE_PISCES config. Get the namelists from `ORCA2_ICE-JZSLX.06` where it was done already.
 

