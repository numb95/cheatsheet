# R

### Read `csv` file

* Use `read.table`  with address as first argument and let `sep` equal to seperator character usually is comma (`sep=","`)
    ```data = read.table("file_address.csv", sep=",")```

    `data` object will a a data frame which you can use it.
    
    * csv file with header
    just add `header = TRUE` argument:
    ``` data = read.table("file_address.csv", sep=",",header=TRUE) ```
    `data` object will dataframe with header names 

