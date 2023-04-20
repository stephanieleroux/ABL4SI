# ORCA2_ICE_ABL NEMO reference config on JeanZay@IDRIS
My step-by-step technical notes on running the ORCA2-ABL1D NEMO reference config on JeanZay@IDRIS.

_Last update: 2023-04-20_

## Install xios

```
cd $WORK/DEV

svn co http://forge.ipsl.jussieu.fr/ioserver/svn/XIOS/trunk@2430 xios-2.5@2430
cd xios-2.5@2430
./make_xios --arch X64_JEANZAY --full --prod --job 8 
```

## Get NEMO4.2 and compile
* Get:

```
cd /gpfswork/rech/cli/regi915/CONFIGS/CONFIG_ORCA2_ICE/

git clone --branch 4.2.0 https://forge.nemo-ocean.eu/nemo/nemo.git ORCA2_ABL-JZSLX.02
```

* Edit arch file:

```
cp arch/CNRS/arch-X64_JEANZAY.fcm arch/CNRS/arch-X64_JEANZAY_slx.fcm  

vi arch/CNRS/arch-X64_JEANZAY_slx.fcm 

# edit: %XIOS_HOME           /gpfswork/rech/cli/regi915/DEV/xios-2.5@2430
```

* Compile:

```
./makenemo -m X64_JEANZAY_slx -r ORCA2_ICE_PISCES -n ORCA2_ABL-JZSLX.02 -j 8
```

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

_Latest status (2023-04-06):_
GS said ORCA2_ICE_ABL is not an official reference config. Should use ORCA2_ICE_PISCES instead (which permits to activate ABL).

_Older comment (2023-04-06):_
Input files from `ORCA2_ABL_v4.2.0.tar.gz` do not seem to match with files needed by the namelist shared with the reference config `ORCA2_ICE_ABL`. Question asked to FL and GS.
