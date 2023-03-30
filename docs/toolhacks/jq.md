# jq


## combine multiple json array to single with unique elements 
```bash
FILES=(`find . -name "<FILEPATTERN>"`)
DATA=$(jq -s 'flatten|sort|unique' $FILES)
echo $DATA > FILENAME.json
```
