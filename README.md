## Overview
Several groups of samples samples have been uploaded to SRA, but need to be organized. The following workflow was created for this task.


## Methods and Code

First, download the metadata file with SRA accessions from https://submit.ncbi.nlm.nih.gov

Then, run the following bash command to parse out the identifiers from the files.  As long as you use a **unique** identifier, such as sample_name, and only search through the downloaded files, the following command will work:


        # copy and paste Baylor IDs from Roger to the file baylor-09_ids
        BAYLOR09=($(<baylor-09_ids))
        for i in "${BAYLOR09[@]}" ; do 
          grep "$i" *.txt >> baylor09_grep.txt ; 
        done
        
        cat baylor10_grep.txt | cut -d$'\t' -f4,5,6 > baylor10_parsed.tsv

Repeat for Baylor-10 and Baylor-11.

Finally, perform a [Full Outer Join](https://www.w3schools.com/sql/sql_join_full.asp) to match the IDs to the SRA data, and determine if any were missed. Excel can be used for this task, as well as R or python (programmatically).  Then modify the [Google Document](https://docs.google.com/spreadsheets/d/11DPbtNG3Y0SPumB0bY-fUOGExG4LXjYB4THpSWp0lLE/edit#gid=0).

An example using pandas is shown here:

        import pandas as pd
        import numpy as np
        
        df_identifiers = pd.read_csv('baylor_10_idList.csv')
        df_SRA = pd.read_csv('baylor10_parsed.tsv', sep='\t')
        merged = pd.merge(df_identifiers, df_SRA, on='Animal_ID', how='outer')
        valueIsNaN = merged[merged['BioSample'].isnull()]
        # valueIsNaN is a data frame with any missing values

## Files
The documents for the outer join are [here](https://github.com/disulfidebond/sort_org_sra_data/blob/master/baylor09_parsed.tsv) and [here](https://github.com/disulfidebond/sort_org_sra_data/blob/master/baylor_09_idList.csv).
The finished document is [here](https://github.com/disulfidebond/sort_org_sra_data/blob/master/baylor_09_outerJoin.csv). 
