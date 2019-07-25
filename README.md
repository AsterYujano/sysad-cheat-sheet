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
Do the same
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


## Useful tools

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

## Useful command for steganography :

eog [path image]
> Open image with image viewer
lsof : liste les processus écoutant sur un port ou un service 

## Sources :
Xavki sysad 
