# SysAdmin Cheat sheet

## List listening process and select which ones to kill (lsof + awk)

```
sudo lsof -i :80 | awk 'NR>1 {print $2}' | xargs sudo kill 
```
Liste les processus écoutant sur le port 80, récupéré le "pid" avec awk, il prend toutes les lignes sauf la première, et récupéré la colonne après 2 tabulations. Puis kill les "pid" listés.

```
lsof -i :80
# List every process listening on port 80

lsof -i :http
# Do the same

awk 'NR>1 {print $2}'
# Takes every lines except the first one.
# print the tabulation number 2 (second column)
 
[...] | xargs [...]
# xargs applies the following command to the arguments, which is `kill` in this case.
```

## Modify pattern/expression on several files (grep + sed)

```
grep -lR "/var/www/html" | xargs sed -i -e "s#/var/www/html#/var/sites#g"
```
Replace "var/www/ in every files containing "/var/www/html"

```
grep -lR "/var/www/html"
# grep -lR for recursevely searching  & gives the files name.

[...] | xargs [...]
# xargs applies the following command to the arguments

sed -i -e "s#/var/www/html#/var/sites#g"
# -i stands for apply the modification
# -e pass command/script/element to the sed command.
# s replace
# g global, apply the modification several times to the same file if the matching string is detected several times.
# Adding the `#` permits to do not escape the `/` character.
```

## Search and archive in a file

```
find . -name "*.sh" | cpio -o --format=tar > compil.tar
```

```
find . -name "*.sh"
# Search from the current directory all .sh files

cpio -o --format=tar > compil.tar
# Deal with archives (Give some files to eat to cpio, it will archive it in the compil.tar file)
```

```
tar -tf compil.tar
# -tf get the archive file and read/list files into it.
```

## HttpServer with Python in 1 line (not secure)

```
python3 -m http.server 9999
#  open web server on the 9999 port
```

```
curl http://example.com:9999/.passwd
wget http://example.com:9999/.passwd
```

## Create a top 10 of your commands based on the history

Useful to know when to use alias with most used commands
```
cat ~/.bash_history | awk `{a[$1" "$2]++}END{for(i in a){print a[i] " " i}}` | sort -nr | head

# {a[$1]++} create a table key-value with the string of the 1st column as key and the number of the string as value.
# in a list with 503 times the command "ls", it would have been "ls-503 times"
# {a[$1" "$2]++} does the same with the value of the column 1 & 2
#
# for(i in a) For each entry in the table, do:
# {print a[i] " " i} Print ls - 504
#
# sort -nr : sort number and reverse
#
# head : takes first 10 entries
```
Or 
```
cat ~/.bash_history | sort | uniq -c | sort -nr | head
```
Which works differently and gives different results


## Useful tools:

### Awk

```
smth.... | awk `{print $1}`
# Print first column of "smth"

smth.... | awk `{print $1" "$2}`
# Print first & second column of "smth"

smth.... | awk `NR>1 {print $1}`
# NR>1 select every line with id more than 1 (which means, not the 1st one).
# and print the first tab.
```

### Grep
```
grep ...
-n : Show the matching number line
--color: highlight the expression
-i : Performing case-insensitive grep searches
-l : list files

grep "something" *.html
    Search "something" in all html files
    
grep -r "something" *
    Search recursively in all files
    Using -R will search though directories symbolically linked
```

### Tree
```
tree
```
show directories and files as a tree.

### Type

```
type [...]

type ls
# indicate if the command is intern or not.
```


## Useful command for steganography:

eog [path image]
> Open image with image viewer
lsof : liste les processus écoutant sur un port ou un service 

## Sources:
Xavki sysad 
