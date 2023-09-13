# My journal 


## 2023-09-08
* Updated NANUK4 with local source from LB for BBM rheology.
* Adapted namelist and xml files.
* first tests: BBM-bulk(std) and BBM-ABL.
* first notebooks on JZ.
  
## 2023-09-01
* Recompiled code of NANUK4 including ABL module
* Run NANUK4 with ABL on and 2013 forcing from GS (`/TEST_NANUK4_4.2/EVPSLX006`)

## 2023-08-29
* Prep BDY and IC for 2012 and 2013 with LB on Frazilo.

## 2023-08-18/25
* First attempt at using GS's 3D forcing files with the NANUK4 config. New weights were computed (interp ERAi to NANUK4).
* Works with `ln_abl=.false.` ! (`TEST_NANUK4_4.2/EVPSLX003`)
* Still need to add some code in prep script so that the weight files and erai-3D files are linked too (and cheat for the 1996 year...)
* Next step to try in `EVPSLX004`: `ln_abl=.true.` same as in namelist ORCA2.
* List of questions to LB.

## 2023-08-18/24
* Worked on getting NANUK4 (NEMOv4.2.1) EVP to work on JZ. --> first success ! (https://github.com/stephanieleroux/nemo_conf_manager/tree/master/TEST_RUN/NANUK4/TEST_NANUK4_4.2/EVPSLX).

## 2023-07-25
* Got infos for NANUK4 config and nemo_conf_manager from LB. 

## 2023-07-21/24
* Re-do tests with ORCA2. Set version to 4.2.1 and try with the forcing files from GS (`/gpfswork/rech/cli/regi915/CONFIGS/CONFIG_ORCA2_ICE/nemo_4.2.1_test/cfgs/ORCA2_JZSLX.10`). Still not sure how to use surface pressure gradient file thought.

## 2023-05-31
* Emails with GS. Got forcing files. Additional info about how these forcing files are made.

## 2023-04-25/27
* Paper reading about  ocean ABL, bulk formulations, and the ABL1D model (in particular: Brodeau et al 2017, Renault et al 2016, Samson 2020 EGU slides, Lemarié et al 2021).

## 2023-04-21
*  Q&A by email with GS about  SETTE and the tests related to switching on  ABL in ORCA2-ICE-PISCES reference config.
* Success! The test with ORCA2-ICE-PISCES ref config with `ln_abl = .true.` ran successfully for 2 months.
* Updated my step-by-step technical notes [here](https://github.com/stephanieleroux/ABL4SI/blob/main/DOCS/ORCA2-ABL1D_NEMO-REF-CONFIG_technotes.md).

## 2023-04-19
* Q&A by email with GS about  ORCA2 config with ABL.

## 2023-04-14
* Asked reference to Einar about their in-house ABL model from Richard Davy. Got the reference of a paper by Richard D. and also his thesis where the model is described (initially developed for the Martian atmosphere and dust).

## 2023-04-05
* Starting with the ORCA2_ICE_ABL  NEMO4.2 reference config on JeanZay@IDRIS.
* My step-by-step technical notes [here](https://github.com/stephanieleroux/ABL4SI/blob/main/DOCS/ORCA2-ABL1D_NEMO-REF-CONFIG_technotes.md).

## 2023-02-10
* My [notes about the existing bibliography](https://github.com/stephanieleroux/SEA-ICE-notes/blob/main/biblio-ABL-interaction.md) on related topics.

## 2023-02-01
* First meeting with FL, GS, LB, PR and SL.

* Overall goal of the project: 

  * use NEMO+SI3 coupled to ABL1D in a realistic arctic configuration,
  * investigate interactions ABL - sea ice from the point of view of sea ice (i.e. study the feedbacks on the ice from an interative ABL) with a BBM rheology.
