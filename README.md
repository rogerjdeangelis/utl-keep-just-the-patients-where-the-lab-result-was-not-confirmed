# utl-keep-just-the-patients-where-the-lab-result-was-not-confirmed
Keep just the patients where the lab result was not confirmed
    SAS Forum: Keep just the patients where the lab result was not confirmed

    github
    https://tinyurl.com/u67za6y
    https://github.com/rogerjdeangelis/utl-keep-just-the-patients-where-the-lab-result-was-not-confirmed

    SAS Forum  very closely related (could use a view to split doce and result)
    https://tinyurl.com/t7hzhg6
    https://communities.sas.com/t5/New-SAS-User/Extract-Specif-observation-from-multiple-observation/m-p/614200


    Remove Patients where the result of the test was not repeated


            Two solutions

                  `a. Sort and datastep
                   b. Prefered -Single Proc summary can provide the repeat number other procs cannot?
                   c. Proc freq but no repeat number (or proc report with dataset output

    data have;
    input Sid $ test $ repeat $ Res $24.;
    cards4;
    1001 WBC 01 WITHIN_NOMAL_RANGE
    1001 WBC 02 WITHIN_NOMAL_RANGE
    1001 WBC 03 WITHIN_NOMAL_RANGE
    1001 WBC 04 OUTSIDE_NORMAL_RANGE
    1001 WBC 05 OUTSIDE_NORMAL_RANGE
    1002 WBC 01 OUTSIDE_NORMAL_RANGE
    1002 WBC 02 WITHIN_NOMAL_RANGE
    1002 WBC 03 WITHIN_NOMAL_RANGE
    1002 WBC 04 WITHIN_NOMAL_RANGE
    1002 WBC 05 WITHIN_NOMAL_RANGE
    1002 RBC 01 WITHIN_NOMAL_RANGE
    1002 RBC 02 WITHIN_NOMAL_RANGE
    1002 RBC 03 WITHIN_NOMAL_RANGE
    1002 RBC 04 OUTSIDE_NORMAL_RANGE
    1002 RBC 05 OUTSIDE_NORMAL_RANGE
    1003 RBC 01 OUTSIDE_NORMAL_RANGE
    1003 RBC 02 WITHIN_NOMAL_RANGE
    1003 RBC 03 WITHIN_NOMAL_RANGE
    1003 RBC 04 WITHIN_NOMAL_RANGE
    1003 RBC 05 WITHIN_NOMAL_RANGE
    ;;;;
    run;quit;

    proc sort data=have out=havSrt;
    by sid test repeat res;
    run;quit;

    *           _
     _ __ _   _| | ___  ___
    | '__| | | | |/ _ \/ __|
    | |  | |_| | |  __/\__ \
    |_|   \__,_|_|\___||___/

    ;

    * sorted to demostrate the rule

    WORK.HAVSRT total obs=20

    Obs    SID     TEST    REPEAT            RES

      1    1001    WBC       01      WITHIN_NOMAL_RANGE     Repeated rusults -> remove
      2    1001    WBC       02      WITHIN_NOMAL_RANGE
      3    1001    WBC       03      WITHIN_NOMAL_RANGE
      4    1001    WBC       04      OUTSIDE_NORMAL_RANGE
      5    1001    WBC       05      OUTSIDE_NORMAL_RANGE

      6    1002    RBC       01      WITHIN_NOMAL_RANGE
      7    1002    RBC       02      WITHIN_NOMAL_RANGE
      8    1002    RBC       03      WITHIN_NOMAL_RANGE
      9    1002    RBC       04      OUTSIDE_NORMAL_RANGE
     10    1002    RBC       05      OUTSIDE_NORMAL_RANGE

     11    1002    WBC       01      OUTSIDE_NORMAL_RANGE   Keep non-repeated test
     12    1002    WBC       02      WITHIN_NOMAL_RANGE
     13    1002    WBC       03      WITHIN_NOMAL_RANGE
     14    1002    WBC       04      WITHIN_NOMAL_RANGE
     15    1002    WBC       05      WITHIN_NOMAL_RANGE


     16    1003    RBC       01      OUTSIDE_NORMAL_RANGE   Keep non-repeated test
     17    1003    RBC       02      WITHIN_NOMAL_RANGE
     18    1003    RBC       03      WITHIN_NOMAL_RANGE
     19    1003    RBC       04      WITHIN_NOMAL_RANGE
     20    1003    RBC       05      WITHIN_NOMAL_RANGE


    * unsorted input

    WORK.HAVE total obs=20

    Obs    SID     TEST    REPEAT            RES

      1    1001    WBC       01      WITHIN_NOMAL_RANGE
      2    1001    WBC       02      WITHIN_NOMAL_RANGE
      3    1001    WBC       03      WITHIN_NOMAL_RANGE
      4    1001    WBC       04      OUTSIDE_NORMAL_RANGE
      5    1001    WBC       05      OUTSIDE_NORMAL_RANGE
      6    1002    WBC       01      OUTSIDE_NORMAL_RANGE
      7    1002    WBC       02      WITHIN_NOMAL_RANGE
      8    1002    WBC       03      WITHIN_NOMAL_RANGE
      9    1002    WBC       04      WITHIN_NOMAL_RANGE
     10    1002    WBC       05      WITHIN_NOMAL_RANGE
     11    1002    RBC       01      WITHIN_NOMAL_RANGE
     12    1002    RBC       02      WITHIN_NOMAL_RANGE
     13    1002    RBC       03      WITHIN_NOMAL_RANGE
     14    1002    RBC       04      OUTSIDE_NORMAL_RANGE
     15    1002    RBC       05      OUTSIDE_NORMAL_RANGE
     16    1003    RBC       01      OUTSIDE_NORMAL_RANGE
     17    1003    RBC       02      WITHIN_NOMAL_RANGE
     18    1003    RBC       03      WITHIN_NOMAL_RANGE
     19    1003    RBC       04      WITHIN_NOMAL_RANGE
     20    1003    RBC       05      WITHIN_NOMAL_RANGE


    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WORK.WANT total obs=2

    Obs    SID     TEST    REPEAT            RES

     1     1002    WBC       01      OUTSIDE_NORMAL_RANGE
     2     1003    RBC       01      OUTSIDE_NORMAL_RANGE

    *                         _         _       _            _
      __ _     ___  ___  _ __| |_    __| | __ _| |_ __ _ ___| |_ ___ _ __
     / _` |   / __|/ _ \| '__| __|  / _` |/ _` | __/ _` / __| __/ _ \ '_ \
    | (_| |_  \__ \ (_) | |  | |_  | (_| | (_| | || (_| \__ \ ||  __/ |_) |
     \__,_(_) |___/\___/|_|   \__|  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                                                                    |_|
    ;

    proc sort data=have out=havSrt;
      by sid test res;
    run;quit;

    data want;
      set havSrt;
      by sid test res;
      if first.res and last.res then output;
    run;quit;

    *_
    | |__     ___ _   _ _ __ ___  _ __ ___   __ _ _ __ _   _
    | '_ \   / __| | | | '_ ` _ \| '_ ` _ \ / _` | '__| | | |
    | |_) |  \__ \ |_| | | | | | | | | | | | (_| | |  | |_| |
    |_.__(_) |___/\__,_|_| |_| |_|_| |_| |_|\__,_|_|   \__, |
                                                       |___/
    ;
    proc summary data=have nway;
      class sid test res;
      id repeat;
      output out=want(drop=_type_ where=(_freq_=1));
    run;quit;

    Up to 40 obs from WANT total obs=2

    Obs    SID     TEST            RES             REPEAT    _FREQ_

     1     1002    WBC     OUTSIDE_NORMAL_RANGE      01         1
     2     1003    RBC     OUTSIDE_NORMAL_RANGE      01         1


    *          __
      ___     / _|_ __ ___  __ _
     / __|   | |_| '__/ _ \/ _` |
    | (__ _  |  _| | |  __/ (_| |
     \___(_) |_| |_|  \___|\__, |
                              |_|
    ;


    proc freq data=have;
    tables sid*test*res / list out=want(drop=percent where=(count=1));
    run;quit;


    Up to 40 obs from WANT total obs=2

    Obs    SID     TEST            RES             COUNT

     1     1002    WBC     OUTSIDE_NORMAL_RANGE      1
     2     1003    RBC     OUTSIDE_NORMAL_RANGE      1


