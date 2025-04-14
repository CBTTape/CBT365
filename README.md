# CBT365
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 365 IS FROM RON MACRAE OF AMDAHL, UK.  THIS FILE CONTAINS *   FILE 365
//*           A SYSTEM TO AUTOMATICALLY PACKAGE ONE OR MORE MVS     *   FILE 365
//*           DATASETS FOR TRANSMISSION ACROSS THE INTERNET,        *   FILE 365
//*           WITH BUILT-IN ERROR DETECTION.  TSO XMIT IS USED      *   FILE 365
//*           UNDER THE COVERS, AND 8 ERROR CHECKING BYTES ARE      *   FILE 365
//*           APPENDED TO EACH 80-BYTE RECORD, SO THAT IF ANY BYTE  *   FILE 365
//*           IS ALTERED DURING THE TRANSMISSION, THE ERROR WILL    *   FILE 365
//*           BE CAUGHT.  YOU'RE GUARANTEED THE INTEGRITY OF YOUR   *   FILE 365
//*           TRANSMITTED FILES.  THE PACKAGE ALSO MAKES IT EASIER  *   FILE 365
//*           TO PACKAGE AND UNPACKAGE MULTIPLE FILES.  ONLY ONE    *   FILE 365
//*           BIG FILE IS ACTUALLY TRANSMITTED.  THE COMBINED FILE  *   FILE 365
//*           FOR TRANSMISSION IS VERY SIMPLE TO CREATE ON THE      *   FILE 365
//*           TRANSMITTING MVS SYSTEM, AND IT IS VERY SIMPLE TO     *   FILE 365
//*           UNWRAP ON THE RECEIVING MVS SYSTEM.  YOU DON'T HAVE   *   FILE 365
//*           TO SPEND A LOT OF TIME FOOLING WITH TSO XMIT COMMAND  *   FILE 365
//*           PARAMETERS.                                           *   FILE 365
//*                                                                 *   FILE 365
//*           THE IBM COMPRESSION UTILITY CALLED TRSMAIN, THAT IS   *   FILE 365
//*           DISTRIBUTED FOR FREE ON THE WEB, IS OPTIONALLY        *   FILE 365
//*           INTEGRATED WITH THIS PROCESS, PROVIDED TRSMAIN IS     *   FILE 365
//*           PRESENT.  IF THE FILES ARE LARGE, IT MAY PAY TO       *   FILE 365
//*           SEND THE MORE COMPRESSED VERSION ACROSS THE INTERNET. *   FILE 365
//*                                                                 *   FILE 365
//*           FILES PRODUCED BY THIS PACKAGE ARE SUFFIXED .XMT .    *   FILE 365
//*           COMPRESSED FILES PRODUCED BY TRSMAIN, ACTING AGAINST  *   FILE 365
//*           THE .XMT FILE, ARE SUFFIXED .XM1 .                    *   FILE 365
//*                                                                 *   FILE 365
//*           THE PACKAGE BASICALLY CONSISTS OF TWO REXX EXECS,     *   FILE 365
//*           AND OPTIONALLY AN ASSEMBLER PROGRAM.  THE EXEC        *   FILE 365
//*           CALLED OSTARXMT WILL PACKAGE ANY NUMBER OF FILES      *   FILE 365
//*           INTO TSO XMIT FORMAT, AND WILL BUNDLE ALL THE FILES   *   FILE 365
//*           TOGETHER, INTO ONE FILE THAT HAS THE BUILT-IN ERROR   *   FILE 365
//*           CHECKING.  OPTIONALLY, THE COMPRESSION UTILITY        *   FILE 365
//*           TRSMAIN WILL BE CALLED AFTERWARD, TO SQUEEZE THE      *   FILE 365
//*           FILE DOWN FURTHER.  IF THE ORIGINAL AMOUNT OF DATA    *   FILE 365
//*           IS LARGE, THIS HELPS.                                 *   FILE 365
//*                                                                 *   FILE 365
//*           THE OTHER EXEC, OSTARREC, WILL UNWRAP THE FILES       *   FILE 365
//*           CREATED BY OSTARXMT, CHECK TO MAKE SURE THERE ARE     *   FILE 365
//*           NO ERRORS, AND WILL CALL TSO RECEIVE FOR EACH OF THE  *   FILE 365
//*           INCLUDED FILES THAT WERE TRANSMITTED.  YOU'LL GET     *   FILE 365
//*           ALL THE FILES THAT WERE INCLUDED IN THE ORIGINAL      *   FILE 365
//*           BUNDLE.                                               *   FILE 365
//*                                                                 *   FILE 365
//*           OPTIONALLY, THE ASSEMBLER PROGRAM, IF ITS PRESENCE    *   FILE 365
//*           IS DETECTED BY THE REXX EXECS, WILL BE CALLED TO DO   *   FILE 365
//*           THE ERROR DETECTION LOGIC.  ALL OF THIS LOGIC IS      *   FILE 365
//*           ALSO BUILT INTO THE REXX EXECS, BUT IF THE ASSEMBLER  *   FILE 365
//*           PROGRAM IS CALLED, THE LOGIC IS EXECUTED FAR FASTER.  *   FILE 365
//*           THIS CAN MAKE A SIGNIFICANT DIFFERENCE IF LARGE       *   FILE 365
//*           AMOUNTS OF DATA ARE TO BE TRANSFERRED.  THE NAME OF   *   FILE 365
//*           THE ASSEMBLER PROGRAM IS OSTAREDC.                    *   FILE 365
//*                                                                 *   FILE 365
//*           AN IBM WEB SITE FROM WHERE YOU CAN DOWNLOAD THE       *   FILE 365
//*           TRSMAIN UTILITY IS:                                   *   FILE 365
//*                                                                 *   FILE 365
//*        ftp://service.boulder.ibm.com/s390/mvs/tools/packlib     *   FILE 365
//*                                                                 *   FILE 365
//*           (YOU HAVE TO USE LOWER CASE TO GET THIS TO WORK.)     *   FILE 365
//*                                                                 *   FILE 365
//*           THIS LOCATION WAS GOOD AS OF THE TIME OF THIS         *   FILE 365
//*           WRITING.  (03/99)                                     *   FILE 365
//*                                                                 *   FILE 365
//*           I'VE INCLUDED BATCH JCL FOR RUNNING THE TRSMAIN       *   FILE 365
//*           COMPRESSION-DECOMPRESSION UTILITY, AND I'VE PUT       *   FILE 365
//*           IN IBM'S "README" FILE FOR TRSMAIN, AS WELL.  I       *   FILE 365
//*           CAN'T INCLUDE THE TRSMAIN MODULE ITSELF--YOU CAN      *   FILE 365
//*           GET IT FREE, FROM IBM.  (SG - 03/99)                  *   FILE 365
//*                                                                 *   FILE 365
//*           THESE REXX EXECS ARE DESIGNED TO BE EXECUTED FROM     *   FILE 365
//*           AN ISPF 3.4 DATASET LIST, OR THEY CAN BE RUN WITH     *   FILE 365
//*           A COMMAND SYNTAX, OR COMMAND PROMPTS.                 *   FILE 365
//*                                                                 *   FILE 365
//*           AUTHOR  : RON MACRAE.                                 *   FILE 365
//*                                                                 *   FILE 365
//*           ADDRESS : OBJECTSTAR SUPPORT                          *   FILE 365
//*                        AMDAHL UK LTD                            *   FILE 365
//*                        CROMWELL HOUSE                           *   FILE 365
//*                        BARTLEY WAY                              *   FILE 365
//*                        HOOK, HAMPSHIRE                          *   FILE 365
//*                        RG27 9XA, UK                             *   FILE 365
//*                                                                 *   FILE 365
//*             EMAIL :  RON_MACRAE@AMDAHL.COM                      *   FILE 365
//*                                                                 *   FILE 365
//*             PHONE : +44-1252-346379                             *   FILE 365
//*                                                                 *   FILE 365
//* -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -   *   FILE 365
//*                                                                 *   FILE 365
//*   Note from Sam Golob:  I've written a small program called     *   FILE 365
//*        OSTRIP, which will take an OSTARXMT-format file and      *   FILE 365
//*        create a series of ordinary XMIT-format files from       *   FILE 365
//*        it.  This is for emergency use only, if the OSTARREC     *   FILE 365
//*        procedure detects errors, and you still want to          *   FILE 365
//*        salvage some data.  OSTRIP is included in this file.     *   FILE 365
//*                                                                 *   FILE 365
//* -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -   *   FILE 365
//*   AMDAHL DISCLAIMER:                                            *   FILE 365
//*                                                                 *   FILE 365
//*         THIS SOFTWARE IS SUPPLIED BY AMDAHL CORP. FOR           *   FILE 365
//*         THE TRANSMISSION OF MATERIALS BETWEEN OBJECTSTAR        *   FILE 365
//*         SUPPORT AND IT'S CUSTOMERS.  ANY OTHER USE OF           *   FILE 365
//*         THIS SOFTWARE IS AT THE USER'S DISCRETION AND IS        *   FILE 365
//*         NOT SUPPORTED IN ANY WAY BY AMDAHL CORP.                *   FILE 365
//*                                                                 *   FILE 365
//*         THE SOFTWARE IS SUPPLIED AS 'FREEWARE' AND MAY          *   FILE 365
//*         BE USED/MODIFIED BY ANYONE PROVIDED THEY DO NOT         *   FILE 365
//*         THEN SELL IT ON FOR PROFIT OR EXPECT SUPPORT            *   FILE 365
//*         FROM AMDAHL CORP.                                       *   FILE 365
//*                                                                 *   FILE 365
//*         LIMITED SUPPORT MAY IN SOME CASES BE AVAILABLE          *   FILE 365
//*         FROM THE AUTHOR.                                        *   FILE 365
//* -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -   *   FILE 365
//*                                                                 *   FILE 365
```
