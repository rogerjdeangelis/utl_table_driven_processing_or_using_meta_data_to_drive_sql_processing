# utl_table_driven_processing_or_using_meta_data_to_drive_sql_processing
Table driven processing or using meta data to drive sql processing.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Table driven processing or using meta data to drive sql processing

    Problem
     Create three tables using sashelp.class where name begins with 'A', 'J' and 'R' respectively

    github
    https://tinyurl.com/ya8ol453
    https://github.com/rogerjdeangelis/utl_table_driven_processing_or_using_meta_data_to_drive_sql_processing

    SAS Forum
    https://tinyurl.com/y75t46yn
    https://communities.sas.com/t5/SAS-Programming/How-to-loop-through-a-proc-sql-select-statement-result/m-p/493556

    Irealize there are many easier ways to do this.

    INPUT (meta data)
    =================

      WORK.CLASS total obs=3

         NAME     SEX    AGE    HEIGHT    WEIGHT

        Alfred     M      14     69.0      112.5
        James      M      12     57.3       83.0
        Robert     M      12     64.8      128.0

    WANT
    ====

      WORK.LOG total obs=3

           SUBSET        RC     STATUS

       Table A _names     0    Completed
       Table J _names     0    Completed
       Table R _names     0    Completed


      WORK.A_NAMES total obs=2

        NAME     SEX    AGE    HEIGHT    WEIGHT

       Alfred     M      14     69.0      112.5
       Alice      F      13     56.5       84.0


      WORK.J_NAMES total obs=7

       NAME       SEX    AGE    HEIGHT    WEIGHT

       James       M      12     57.3       83.0
       Jane        F      12     59.8       84.5
       Janet       F      15     62.5      112.5
       Jeffrey     M      13     62.5       84.0
       John        M      12     59.0       99.5
       Joyce       F      11     51.3       50.5
       Judy        F      14     64.3       90.0


      WORK.R_NAMES total obs=2

        NAME     SEX    AGE    HEIGHT    WEIGHT

       Robert     M      12     64.8       128
       Ronald     M      15     67.0       133


    PROCESS
    =======

    data log;

      retain subset;

      set class;

      chr1st=substr(name,1,1);

      call symputx('chr1st',chr1st);

      subset=catx(' ','Table',chr1st,'_names');

      rc=dosubl('
        proc sql;
          create
             table &chr1st._names as
          select
             *
          from
             sashelp.class
          where
             name eqt "&chr1st";
         quit;
         %let cc=&syserr;
      ');

      if symgetn('cc') = 0 then status="Completed";
      else status="Failed";

      keep subset rc status;

    run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data class;
       set sashelp.class;
       if _n_=1 or _n_=6 or _n_=16;
    run;quit;

