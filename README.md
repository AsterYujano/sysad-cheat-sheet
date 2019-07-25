# SysAdmin Cheat sheet

## List listening process and select which ones to kill (lsof + awk)

```
sudo lsof -i :80 | awk 'NR>1 {print &2}' | xargs sudo kill 
```
Liste les processus écoutant sur le port 80, récupéré le "pid" avec awk, il prend toutes les lignes sauf la première, et récupéré la colonne après 2 tabulations. Puis kill les "pid" listés.

```
lsof -i :80
# List every process listening on port 80

lsof -i :http
# Do the same

awk 'NR>1 {print &2}'
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

## Useful tools:

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
