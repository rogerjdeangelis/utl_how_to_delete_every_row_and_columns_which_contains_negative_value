# utl_how_to_delete_every_row_and_columns_which_contains_negative_value
How to Delete Every Row and Columns Which Contains Negative Value Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    How to Delete Every Row and Columns Which Contains Negative Value

    Not so elegant in a non matrix language

    github
    https://goo.gl/iUNq1Z
    https://github.com/rogerjdeangelis/utl_how_to_delete_every_row_and_columns_which_contains_negative_value

    INPUT
    =====

     SD1.HAVE total obs=5           |   RULES
                                    |
        C1    C2    C3    C4    C5  |   Delete columns C1, C3 and C4 due to -1
                                    |   Delete rows R1, R3 and R5
    1   -1     2     0     1     2  |
    2    2     1     0     2     1  |    C2    C5
    3    0     0     0    -1     0  |
    4    1     2     0     1     2  |     1     1
    5    2     1    -1     2     1  |     2     2


    WORKING CODE                                       * DROPS;
                                                 C1          C3   C4
        col_index <- !colSums(have < 0);      * FALSE TRUE FALSE FALSE TRUE;
                                                 R1          R3         R5
        row_index <- !rowSums(have < 0);      * FALSE TRUE FALSE TRUE  FALSE;

        want <- have[row_index, col_index];   * only the trues will survive;

    OUTPUT
    ======

     WORK.WANT total obs=2

        C2    C5

         1     1
         2     2

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have(drop=row col);

       array cols[*] c1-c5;
       do row=1 to dim(cols);
          do col=1 to dim(cols);
             if uniform(5731)<.08 then cols[col]=-1;
             else cols[col]=mod(row*col,3);
          end;
          output;
       end;

    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    col_index <- !colSums(have < 0);
    row_index <- !rowSums(have < 0);
    want <- have[row_index, col_index];
    endsubmit;
    import r=want data=wrk.want;
    run;quit;
    ');

    proc print data=want;
    run;quit;

    see
    https://goo.gl/rDNrB4
    https://stackoverflow.com/questions/48005234/how-to-delete-every-rowcolumns-which-contains-negative-value

    Jaap profile
    https://stackoverflow.com/users/2204410/jaap


    I'd like to purge every row and column if any element in that column or row holds negative value.


    1. colSums(d < 0) gives a numeric vector of the
       number of negative values in the columns.
    2. By negating it with ! you create a logical vector where for the
       columns with no negative values get a TRUE value.
    3. It works the same for rows.
    4. Subsetting the dataframe with the row_index and the col_index gives you a
       dataframe where the rows as wel as the columns where
       the negative values appeared are removed.


