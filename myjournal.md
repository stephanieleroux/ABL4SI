# My journal 

## 2023-08-18-24
* Worked on getting NANUK4 (NEMOv4.2.1) EVP to work on JZ. --> success. See notes.

## 2023-07-25
* Got infos for NANUK4 config from LB. 

## 2023-07-21/24
* Re-do tests with ORCA2. Set version to 4.2.1 and try with the forcing files from GS. Still not sure how to use surface pressure gradient file thought.

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
