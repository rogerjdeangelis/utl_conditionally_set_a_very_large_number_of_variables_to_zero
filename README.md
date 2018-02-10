# utl_conditionally_set_a_very_large_number_of_variables_to_zero
Conditionally set a very large number of variables to zero. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Conditionally set a very large number of variables to zero

    github
    https://goo.gl/kFhMTC
    https://github.com/rogerjdeangelis/utl_conditionally_set_a_very_large_number_of_variables_to_zero

    see
    https://goo.gl/9F2aGg
    https://communities.sas.com/t5/General-SAS-Programming/Conditionally-Replacing-Missing-Values-with-0/m-p/435857


    INPUT
    =====

    ALGORITHM
        When sex='M' set the 200 variables ABCD1 -- WXYZ100 to 0


    WORK.HAVE (sashelp.class name, sex and 200 variables with value 1)

       Middle Observation(9 ) of Last dataset = WORK.HAVE - Total Obs 19

        -- CHARACTER --            --VALUE--

       NAME                C8       Jeffrey
       SEX                 C1       M


        -- NUMERIC -- (200 variables)
       ABCD1               N8       1
       ABCD2               N8       1
       ABCD3               N8       1
       ABCD4               N8       1
       ABCD5               N8       1

       EFGH1               N8       1
       EFGH2               N8       1
       EFGH3               N8       1
       EFGH4               N8       1
       EFGH5               N8       1

       IJKL1               N8       1
       IJKL2               N8       1
       IJKL3               N8       1
       ......
       IJKL88              N8       1
       IJKL89              N8       1
       IJKL90              N8       1

       WXYZ1               N8       1
       WXYZ2               N8       1
       WXYZ3               N8       1
       .....
       WXYZ98              N8       1
       WXYZ99              N8       1
       WXYZ100             N8       1


    PROCESS
    =======

      * note data is not contiguous in permanent arrays however should not be a problem
        for arrays with lengths less than pasgsize, usually 64k?;
      * data is contiguous in temporary arrays;

      data want;
        set have;
        set sashelp.class(keep=name sex);
        array nums[200] ABCD1-ABCD5 EFGH1-EFGH5 IJKL1-IJKL90 WXYZ1-WXYZ100 (200*1);;
        if sex="M" then call pokelong (repeat (put (0,rb8.),200), addrlong(abcd1));
      run;quit;


    OUTPUT  (partial listing)
    =========================

    Females unchanged but males are all 0

    WORK,WANT total obs=19

    Obs NAME    SEX ZRO ABCD1 ABCD2 ABCD3 ABCD4 ABCD5 EFGH1 EFGH2 EFGH3 EFGH4 EFGH5 IJKL1 IJKL2 ....

      1 Alfred   M   0     0     0     0     0     0     0     0     0     0     0     0     0
      2 Alice    F   1     1     1     1     1     1     1     1     1     1     1     1     1
      3 Barbara  F   1     1     1     1     1     1     1     1     1     1     1     1     1
      4 Carol    F   1     1     1     1     1     1     1     1     1     1     1     1     1
      5 Henry    M   0     0     0     0     0     0     0     0     0     0     0     0     0
      6 James    M   0     0     0     0     0     0     0     0     0     0     0     0     0
      7 Jane     F   1     1     1     1     1     1     1     1     1     1     1     1     1

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
      retain name sex;
      array nums[200] ABCD1-ABCD5 EFGH1-EFGH5 IJKL1-IJKL90 WXYZ1-WXYZ100 (200*1);
      set sashelp.class(keep=name sex);
    run;quit;


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    *SAS;
      data want;
        set have;
        set sashelp.class(keep=name sex);
        array nums[200] ABCD1-ABCD5 EFGH1-EFGH5 IJKL1-IJKL90 WXYZ1-WXYZ100 (200*1);;
        if sex="M" then call pokelong (repeat (put (0,rb8.),200), addrlong(abcd1));
      run;quit;


    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data wrk.wantwps;
      set wrk.have;
      array nums[200] ABCD1-ABCD5 EFGH1-EFGH5 IJKL1-IJKL90 WXYZ1-WXYZ100 (200*1);;
      if sex="M" then call pokelong (repeat (put (0,rb8.),200), addrlong(abcd1));
    run;quit;
    ');

