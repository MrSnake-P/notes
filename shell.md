` cat transaction.csv | csvtk head -n 50 | csvtk filter -f "3=-1" | csvtk cut -f 1,2,4-6,17,19 > /home/jifengfeicui/ethParity/output.csv`

`cat transaction.csv | csvtk head -n 20 | csvtk -H filter -f "2=-1" | csvtk cut -f 1,3-6,9-10 > /home/jifengfeicui/ethParity/input.csv`

分割分件

```shell
split -l 2 test.csv -d -a 1 addr && ls | grep addr | xargs -n1 -i{} mv {} {}.csv
```

1432961890

