# utl-three-dimensional-crosstab-proc-freq-tabulate-corresp-and-report
Three dimensional crosstab proc freq tabulate corresp and report

    Three dimensional crosstab proc freq tabulate corresp and report                                                     
                                                                                                                         
    https://github.com/rogerjdeangelis/utl-three-dimensional-crosstab-proc-freq-tabulate-corresp-and-report              
                                                                                                                         
    https://communities.sas.com/t5/SAS-Procedures/Help-with-generating-a-complex-table/m-p/591175                        
                                                                                                                         
      Four Solutions (all produce a sas dataset not a static printout)                                                   
                                                                                                                         
         Require normailzation step                                                                                      
                                                                                                                         
            a. proc corresp                                                                                              
            b. proc report                                                                                               
            c. proc freq                                                                                                 
                                                                                                                         
         Does not require normalization but does require intermediate excel file                                         
                                                                                                                         
            d. Proc tabulate to execl to sas dataset (does not require normailzation)                                    
                                                                                                                         
               This is a method to fix the ods bug with proc tabulate.                                                   
               The tabulate ods output dataset looks nothing like the listing output..                                   
               It is interesting that ods excel output is what ods output should be.                                     
                                                                                                                         
    FYI                                                                                                                  
       Ods excel can be used as an intermedia to work around the many SAS procedures                                     
       where the ods output SAS dataset does not look anything like the ods listing.                                     
                                                                                                                         
    SOAPBOX ON                                                                                                           
                                                                                                                         
      We have IML Studio                                                                                                 
                                                                                                                         
       Maybe we need                                                                                                     
         Proc freq studio                                                                                                
         Proc tabulate studio                                                                                            
         Proc report studio                                                                                              
       to fix all those ods bugs?                                                                                        
                                                                                                                         
      It is interesting that 'proc corresp' is able to match frquency listings.                                          
    SOAPBOX OFF;                                                                                                         
                                                                                                                         
    *_                   _                                                                                               
    (_)_ __  _ __  _   _| |_                                                                                             
    | | '_ \| '_ \| | | | __|                                                                                            
    | | | | | |_) | |_| | |_                                                                                             
    |_|_| |_| .__/ \__,_|\__|                                                                                            
            |_|                                                                                                          
    ;                                                                                                                    
                                                                                                                         
    data have;                                                                                                           
     input id ctr$ Vr1$ Vr2$ Vr3$ vr4$ vr5$;                                                                             
    cards4;                                                                                                              
    1 c1 1 0 0 0 1                                                                                                       
    1 c2 0 0 1 0 1                                                                                                       
    1 c3 0 1 0 0 1                                                                                                       
    1 c4 0 0 1 0 1                                                                                                       
    1 c5 1 1 1 0 1                                                                                                       
    1 c1 1 1 1 1 0                                                                                                       
    1 c2 1 1 0 1 0                                                                                                       
    1 c3 1 0 1 1 0                                                                                                       
    1 c4 1 0 0 1 0                                                                                                       
    1 c5 0 0 0 1 0                                                                                                       
    2 c1 1 1 1 1 0                                                                                                       
    2 c2 1 1 1 1 0                                                                                                       
    2 c3 1 0 1 1 0                                                                                                       
    2 c4 1 0 0 0 0                                                                                                       
    2 c5 0 1 1 1 0                                                                                                       
    2 c1 0 0 0 0 1                                                                                                       
    2 c2 1 1 1 1 1                                                                                                       
    2 c3 0 1 0 1 1                                                                                                       
    2 c4 0 1 1 1 1                                                                                                       
    2 c5 1 1 1 0 1                                                                                                       
    ;;;;                                                                                                                 
    run;quit;                                                                                                            
                                                                                                                         
    WORK.HAVE total obs=20                                                                                               
                                                                                                                         
     ID    CTR    VR1    VR2    VR3    VR4    VR5                                                                        
                                                                                                                         
      1    c1      1      0      0      0      1                                                                         
      1    c2      0      0      1      0      1                                                                         
      1    c3      0      1      0      0      1                                                                         
      1    c4      0      0      1      0      1                                                                         
      1    c5      1      1      1      0      1                                                                         
      1    c1      1      1      1      1      0                                                                         
     ....                                                                                                                
                                                                                                                         
    *           _                                                                                                        
     _ __ _   _| | ___  ___                                                                                              
    | '__| | | | |/ _ \/ __|                                                                                             
    | |  | |_| | |  __/\__ \                                                                                             
    |_|   \__,_|_|\___||___/                                                                                             
                                                                                                                         
    ;                                                                                                                    
                                                                                                                         
    * If you "normalize" have it becomes obvious how to solve the problem;                                               
    * Note you do not need this step with the proc tabulate/execl solution;                                              
                                                                                                                         
    data havNrm;                                                                                                         
     length var ;                                                                                                        
     set have;                                                                                                           
     array vs vr:;                                                                                                       
     do over vs;                                                                                                         
       var=catx("-",vname(vs),vs);                                                                                       
       drop id vr: ctr;                                                                                                  
       output;                                                                                                           
     end;                                                                                                                
    run;quit;                                                                                                            
                                                                                                                         
    NOW it is a simple crosstab                                                                                          
                                                                                                                         
    WORK.HAVNRM total obs=100                                                                                            
                                                                                                                         
       VAR     CENTER                                                                                                    
                                                                                                                         
      VR1-1    ctr-1                                                                                                     
      VR2-0    ctr-1                                                                                                     
      VR3-0    ctr-1                                                                                                     
      VR4-0    ctr-1                                                                                                     
      VR5-1    ctr-1                                                                                                     
      VR1-0    ctr-2                                                                                                     
      VR2-0    ctr-2                                                                                                     
      VR3-1    ctr-2                                                                                                     
      VR4-0    ctr-2                                                                                                     
      VR5-1    ctr-2                                                                                                     
      VR1-0    ctr-3                                                                                                     
      VR2-1    ctr-3                                                                                                     
      VR3-0    ctr-3                                                                                                     
      VR4-0    ctr-3                                                                                                     
      VR5-1    ctr-3                                                                                                     
     ...                                                                                                                 
                                                                                                                         
    *            _               _                                                                                       
      ___  _   _| |_ _ __  _   _| |_                                                                                     
     / _ \| | | | __| '_ \| | | | __|                                                                                    
    | (_) | |_| | |_| |_) | |_| | |_                                                                                     
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                    
                    |_|                                                                                                  
      __ _       ___ ___  _ __ _ __ ___  ___ _ __                                                                        
     / _` |     / __/ _ \| '__| '__/ _ \/ __| '_ \                                                                       
    | (_| |_   | (_| (_) | |  | | |  __/\__ \ |_) |                                                                      
     \__,_(_)   \___\___/|_|  |_|  \___||___/ .__/                                                                       
    ;                                                                                                                    
                                                                                                                         
    WORK.WANTCOR total obs=11                                                                                            
                                                                                                                         
       LABEL    CTR_1    CTR_2    CTR_3    CTR_4    CTR_5    SUM                                                         
                                                                                                                         
       VR1-0       1        1        2        2        2       8                                                         
       VR1-1       3        3        2        2        2      12                                                         
       VR2-0       2        1        2        3        1       9                                                         
       VR2-1       2        3        2        1        3      11                                                         
       VR3-0       2        1        2        2        1       8                                                         
       VR3-1       2        3        2        2        3      12                                                         
       VR4-0       2        1        1        2        2       8                                                         
       VR4-1       2        3        3        2        2      12                                                         
       VR5-0       2        2        2        2        2      10                                                         
       VR5-1       2        2        2        2        2      10                                                         
       Sum        20       20       20       20       20     100                                                         
                                                                                                                         
    *_                                   _                                                                               
    | |__      _ __ ___ _ __   ___  _ __| |_                                                                             
    | '_ \    | '__/ _ \ '_ \ / _ \| '__| __|                                                                            
    | |_) |   | | |  __/ |_) | (_) | |  | |_                                                                             
    |_.__(_)  |_|  \___| .__/ \___/|_|   \__|                                                                            
                       |_|                                                                                               
    ;                                                                                                                    
                                                                                                                         
    WORK.WANTRPT total obs=10                                                                                            
                                                                                                                         
       VAR     CTR_1    CTR_2    CTR_3    CTR_4    CTR_5                                                                 
                                                                                                                         
      VR1-0      1        1        2        2        2                                                                   
      VR1-1      3        3        2        2        2                                                                   
      VR2-0      2        1        2        3        1                                                                   
      VR2-1      2        3        2        1        3                                                                   
      VR3-0      2        1        2        2        1                                                                   
      VR3-1      2        3        2        2        3                                                                   
      VR4-0      2        1        1        2        2                                                                   
      VR4-1      2        3        3        2        2                                                                   
      VR5-0      2        2        2        2        2                                                                   
      VR5-1      2        2        2        2        2                                                                   
                                                                                                                         
    *           __                                                                                                       
      ___      / _|_ __ ___  __ _                                                                                        
     / __|    | |_| '__/ _ \/ _` |                                                                                       
    | (__ _   |  _| | |  __/ (_| |                                                                                       
     \___(_)  |_| |_|  \___|\__, |                                                                                       
                               |_|                                                                                       
    ;                                                                                                                    
                                                                                                                         
    WORK.WANT_FRQXLS total obs=11                                                                                        
                                                                                                                         
     FREQUENCY    CTR_1    CTR_2    CTR_3    CTR_4    CTR_5    TOTAL                                                     
                                                                                                                         
      VR1-0         1        1        2        2        2        8                                                       
      VR1-1         3        3        2        2        2       12                                                       
      VR2-0         2        1        2        3        1        9                                                       
      VR2-1         2        3        2        1        3       11                                                       
      VR3-0         2        1        2        2        1        8                                                       
      VR3-1         2        3        2        2        3       12                                                       
      VR4-0         2        1        1        2        2        8                                                       
      VR4-1         2        3        3        2        2       12                                                       
      VR5-0         2        2        2        2        2       10                                                       
      VR5-1         2        2        2        2        2       10                                                       
      Total        20       20       20       20       20      100                                                       
                                                                                                                         
    *    _      _        _           _       _                                                                           
      __| |    | |_ __ _| |__  _   _| | __ _| |_ ___                                                                     
     / _` |    | __/ _` | '_ \| | | | |/ _` | __/ _ \                                                                    
    | (_| |_   | || (_| | |_) | |_| | | (_| | ||  __/                                                                    
     \__,_(_)   \__\__,_|_.__/ \__,_|_|\__,_|\__\___|                                                                    
                                                                                                                         
    ;                                                                                                                    
                                                                                                                         
     WANTTABXLS total obs=10                                                                                             
                                                                                                                         
       VAR     C1    C2    C3    C4    C5                                                                                
                                                                                                                         
      var-0     1     1     2     2     2                                                                                
      var-1     3     3     2     2     2                                                                                
      var-0     2     1     2     3     1                                                                                
      var-1     2     3     2     1     3                                                                                
      var-0     2     1     2     2     1                                                                                
      var-1     2     3     2     2     3                                                                                
      var-0     2     1     1     2     2                                                                                
      var-1     2     3     3     2     2                                                                                
      var-0     2     2     2     2     2                                                                                
      var-1     2     2     2     2     2                                                                                
                                                                                                                         
    *          _       _   _                                                                                             
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                             
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                                            
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                            
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                            
      __ _       ___ ___  _ __ _ __ ___  ___ _ __                                                                        
     / _` |     / __/ _ \| '__| '__/ _ \/ __| '_ \                                                                       
    | (_| |_   | (_| (_) | |  | | |  __/\__ \ |_) |                                                                      
     \__,_(_)   \___\___/|_|  |_|  \___||___/ .__/                                                                       
                                            |_|                                                                          
    ;                                                                                                                    
                                                                                                                         
    ods exclude all;                                                                                                     
    ods output observed=wantCor ;                                                                                        
    proc corresp data=havNrm dim=1 observed;                                                                             
    tables var, center;                                                                                                  
    run;quit;                                                                                                            
    ods select all;                                                                                                      
                                                                                                                         
    *_                                   _                                                                               
    | |__      _ __ ___ _ __   ___  _ __| |_                                                                             
    | '_ \    | '__/ _ \ '_ \ / _ \| '__| __|                                                                            
    | |_) |   | | |  __/ |_) | (_) | |  | |_                                                                             
    |_.__(_)  |_|  \___| .__/ \___/|_|   \__|                                                                            
                       |_|                                                                                               
    ;                                                                                                                    
                                                                                                                         
    * ods bug - report does not support the ods listing;                                                                 
    proc report data=havNrm nowd out=wantrpt  (rename=(                                                                  
       _C2_ = ctr_1                                                                                                      
       _C3_ = ctr_2                                                                                                      
       _C4_ = ctr_3                                                                                                      
       _C5_ = ctr_4                                                                                                      
       _C6_ = ctr_5));                                                                                                   
    cols var center ;                                                                                                    
    define var / group;                                                                                                  
    define center / across;                                                                                              
    run;quit;                                                                                                            
                                                                                                                         
    *           __                                                                                                       
      ___      / _|_ __ ___  __ _                                                                                        
     / __|    | |_| '__/ _ \/ _` |                                                                                       
    | (__ _   |  _| | |  __/ (_| |                                                                                       
     \___(_)  |_| |_|  \___|\__, |                                                                                       
                               |_|                                                                                       
    ;                                                                                                                    
                                                                                                                         
    ods excel file="d:/xls/want_frq.xlsx" options(sheet_name='tblFrq');;                                                 
    proc freq data=WORK.HAVNRM;                                                                                          
      tables var*center / nopercent nocol norow;                                                                         
    run;quit;                                                                                                            
    ods excel close;;                                                                                                    
                                                                                                                         
    libname xel "d:/xls/want_frq.xlsx"  scantext=no mixed=yes;                                                           
    data want_frqxls;                                                                                                    
      set xel.'tblfrq$A5:Z99'n;                                                                                          
    run;quit;                                                                                                            
    libname xel clear;                                                                                                   
                                                                                                                         
    *    _      _        _           _       _                                                                           
      __| |    | |_ __ _| |__  _   _| | __ _| |_ ___                                                                     
     / _` |    | __/ _` | '_ \| | | | |/ _` | __/ _ \                                                                    
    | (_| |_   | || (_| | |_) | |_| | | (_| | ||  __/                                                                    
     \__,_(_)   \__\__,_|_.__/ \__,_|_|\__,_|\__\___|                                                                    
                                                                                                                         
    ;                                                                                                                    
                                                                                                                         
    * works directly with have does not require normalization;                                                           
                                                                                                                         
    ods excel file="d:/xls/tmp.xlsx" options(sheet_name='table');;                                                       
    proc tabulate data=have missing;                                                                                     
       class v: ctr:;                                                                                                    
       tables vr1="" vr2="" vr3="" vr4="" vr5="", ctr=""*N="";                                                           
    run;                                                                                                                 
    ods excel close;                                                                                                     
                                                                                                                         
    libname xel "d:/xls/tmp.xlsx"  scantext=no mixed=yes;                                                                
    data wanttabxls;                                                                                                     
      retain var "      " v 0;                                                                                           
      set xel.'table$'n;                                                                                                 
      If mod(_n_-1,2)=0 then v=v+1;                                                                                      
      var=catx("-","var",f1);                                                                                            
      drop v f1;                                                                                                         
                                                                                                                         
    run;quit;                                                                                                            
    libname xel clear;                                                                                                   
                                                                                                                         
