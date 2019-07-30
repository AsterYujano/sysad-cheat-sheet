# AWK

## Input

Often used after a command as input. For example:

```
cat myfile.txt | awk ...
```

- $1 : field 1
- $2 : field 2
- ...
- $0 : all fields


### Separator

The default separator in the input is `tabulation`. To modify it (For `CSV` as input for example), use the `- F` flag with the new separator. 

```
awk -F ';' '{print $1}'
```


## Basis

### Concatenate

```
awk '{print $1 $2}'
```

Concatenate with `||` or `+` or `;`

```
awk '{print $1 " || " $2}'
awk '{print $1 "+" $2}'
awk '{print $1 ";" $2}'
```


## Script with AWK

```
#!/usr/bin/awk -f

BEGIN {
print "This will be a script"
}
```
`./myscript.awk`


## AWK Variables

### NF - Field Number

Get the fields/columns number in a row

```
cat table.txt | awk '{print NF " => " $0}'
# print the fields number in each line
```

### NR - Rows Number

Count lines & Give the line number.

```
cat table.txt | awk '{print NR " => " $0}'
# print the line number of each row
```

### END

The last element of the command. Element printed after the last line of the table.

```
cat table.txt | awk 'END {print NR}'
# print the number of row in the table.

cat table.txt | awk '{print $0} END {print "Number of lines: "NR}'
``` 

### BEGIN

Same as END but before the table


## Pattern

To search a pattern, use `/pattern/`

To search anti-pattern, use `!/pattern/`

`length()` gives the number of chars.

example:
```
cat table.txt | awk '
BEGIN {print "\n\nbeginning, lastnames list:\n"};
!/firstname/ {print "lastname: " $2 " - length :" length($2)}
'
```

With regex:
```
/first[na]+me/

# Search lines with a word starting with "first" and finishing with "me", with x times "na" between.
```

## gsub()

To replace a pattern.

```
gsub("old_pattern", "new_pattern", "fields")
```

example:

```
cat names.txt | awk '{gsub("firstname", "prenom", $1); print $0}'
```

## IF - Conditions

if(`condition`) `Do something here` else if(`other condition`) `Another action` else `Last action`

example:
```
cat smth.txt | axk '{ if(NR==1) print $1 ;else if ($2=="Aster") print $0;}'
```

With ternaire:
```
{print NR==2? $2:""}
```

### Example: Analyse Apache Logs with AWK

```
cat /var/log/apache2/access.log |
awk '
{gsub(*\[|:.*","",$4) tab[$4" - "$1]++}
END { for (i in tab) print i "hits:" tab[i] }
'
```

## match()

math(field, /pattern/) : capture a pattern

print substr($0, RSTART, RLENGTH) : print the capture

Example:
```
cat logs.txt |
awk '
match($0, /192(.[0-9]+)+/)
{
print substr($O, RSTART, RLENGTH)
}
'
```

## Variable

no `$` to declare a variable.

```
awk -F "" '
{
print NF;
total = total + NF;
}
END {
print "Number of chars: " total
}
'
```

## Split & For

Array : no `$` and tab[index]

### Split

split(input, tab_name, separator)

```
split($0, tab, " ")
print tab[2]

# does the same as $2
```

### For - Loop

for(i=1;i<=10;i++){print i}

## Delete twin lines

```
cat ... |
awk '
{tab[$0]++}
END{for (line in tab) print line}
'
```

## NEXT - Skip a line 

```
awk '
/word/ {next}
{print $0}
'
# if there is "word" in the whole line, goes next, then print all without next elements.

awk '
$2 ~ /word/ {next}
{print $0}
'
# Chech only the column 2
```

## Using Bash Variable with AWK

`-v` flag with `awk_var="$var_shell"`

example:
```
day=$(date +%d/%Y/%m);
cat myfile.txt | 
awk
-v var="$day"
'{print var " : " $0}'
```

## Example: Generate SQL

```
ps aux |
awk '
!/\[/
{print "INSERT INTO process values ("$1" "$2" "$1");"}
'

# '\''
# To give ' to bash, we need to use '' to leave the awk command line, then escape a single quote.

ps aux |
awk '
!/\[/
{print "INSERT INTO process values ('\''"$1"'\'', '\''"$2"'\'', "$1"'\'');"}
'
```



