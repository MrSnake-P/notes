```python
import csv

lines = []
with open('output.txt','r') as f:
    for line in f.readlines():
        lines.append(line[:-1])

with open('corrected.csv','w') as correct:
    writer = csv.writer(correct, dialect = 'excel')
    with open('input.csv', 'rb') as mycsv:
        reader = csv.reader( (line.replace('\0','') for line in mycsv) )
        for row in reader:
            if row[0] not in lines:
                writer.writerow(row)
```