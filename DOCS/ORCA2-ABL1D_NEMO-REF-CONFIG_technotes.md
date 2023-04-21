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
* Get release 4.2.0:

```
cd /gpfswork/rech/cli/regi915/CONFIGS/CONFIG_ORCA2_ICE/

git clone --branch 4.2.0 https://forge.nemo-ocean.eu/nemo/nemo.git ORCA2_ABL-JZSLX.02
```

* Edit arch file:

```
cp arch/CNRS/arch-X64_JEANZAY.fcm arch/CNRS/arch-X64_JEANZAY_slx.fcm  

vi arch/CNRS/arch-X64_JEANZAY_slx.fcm 

# edit: %XIOS_HOME            /gpfswork/rech/cli/rote001/DEV/xios_trunk_2430
# does nt work yet /gpfswork/rech/cli/regi915/DEV/xios-2.5@2430 so i use xios from AA.
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

```
    NPROC=32
    set_namelist namelist_cfg nn_it000 1
    set_namelist namelist_cfg nn_itend 992
    set_namelist namelist_cfg jpni 4
    set_namelist namelist_cfg jpnj 8
    set_namelist namelist_cfg sn_cfctl%l_runstat .true.
    set_namelist namelist_cfg sn_cfctl%l_trcstat .true.
    set_namelist namelist_cfg ln_wave .true.
    set_namelist namelist_cfg ln_cdgw .false.
    set_namelist namelist_cfg ln_sdw  .true.
    set_namelist namelist_cfg ln_stcor .true.

    set_namelist_opt namelist_cfg ln_icebergs ${USING_ICEBERGS} .true. .false.
    set_namelist_opt namelist_cfg nn_hls ${USING_EXTRA_HALO} 2 1
    set_namelist_opt namelist_cfg nn_comm ${USING_COLLECTIVES} 2 1
    set_namelist_opt namelist_cfg ln_nnogather ${USING_NOGATHER} .true. .false.
    set_namelist_opt namelist_cfg ln_tile ${USING_TILING} .true. .false.
  
    set_namelist namelist_top_cfg ln_trcdta .false.
    set_namelist namelist_top_cfg ln_trcbc  .false.
    # put ln_ironsed, ln_hydrofe to false
    # if not you need input files, and for tests is not necessary
    set_namelist namelist_pisces_cfg ln_varpar .false.
    set_namelist namelist_pisces_cfg ln_ironsed .false.
    set_namelist namelist_pisces_cfg ln_ironice .false.
    set_namelist namelist_pisces_cfg ln_hydrofe .false.
    # put ln_pisdmp to false : no restoring to global mean value
    set_namelist namelist_pisces_cfg ln_pisdmp .false.
    set_namelist_opt namelist_cfg ln_timing ${USING_TIMING} .true. .false.
 ```
 
_Latest status (2023-04-06):_
GS said ORCA2_ICE_ABL is not an official reference config. Should use ORCA2_ICE_PISCES instead (which permits to activate ABL).

_Older comment (2023-04-06):_
Input files from `ORCA2_ABL_v4.2.0.tar.gz` do not seem to match with files needed by the namelist shared with the reference config `ORCA2_ICE_ABL`. Question asked to FL and GS.
