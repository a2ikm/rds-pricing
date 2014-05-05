RDS/MySQL Specs and Prices
========================================

## How to Use

`build/*.csv` are the data.

If you want to see in human-friendly format, `column` command will work good like below:

```plain
$ cat build/ap-northeast-1-multi-az.csv | column -s , -t
DB Instance Class             Instance Type   vCPU  ECU       Memory (GiB)  PIOPS-Optimized  Network Performance  Price Per Hour
Micro instances               db.t1.micro     1     Variable  0.615         -                Very Low             $0.060
Standard - Second Generation  db.m3.medium    1     3         3.75          -                Moderate             $0.240
Standard - Second Generation  db.m3.large     2     6.5       7.5           -                Moderate             $0.480
(snip)
```

## How to Update

`csv/spec.csv` is for specs, and `csv/prices/*.csv` are for prices. Update or add them for your need, and run `rake` to build joined files.

Original data are on https://aws.amazon.com/rds/pricing/ and https://aws.amazon.com/rds/details/ . Adding new data from their tables, you may need to clean up them manually, for example, removing asterisk, matching instance type names, and so on.
