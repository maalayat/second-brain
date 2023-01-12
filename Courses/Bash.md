## ls for each folder
```
ls | xargs -I_ ls _
```

## Filter content with grep
```
cat file.txt | grep d
```

```
cat hi | grep d > filtered.txt
```

### Better performance
```
grep d < hi
```

## Variables

### Local variable
```
variable_name="hi"
echo $variable_name
```

### Global variable (export)
```
export WHEREVER="asdasd"
echo $WHEREVER
```

### Inmutability
```
$ function hi {
> local -r first_arg=$1
> echo "FIRTS ARGUMENT:"
> echo $first_arg
> }
> 
$ hi
FIRTS ARGUMENT:

$ hi xyz
FIRTS ARGUMENT:
xyz
```

## Loops

### for
```
for x in $(seq 1 10); do  
> echo "Hi I'm the number: $x"  
> done  
Hi I'm the number: 1  
Hi I'm the number: 2  
Hi I'm the number: 3  
Hi I'm the number: 4  
Hi I'm the number: 5  
Hi I'm the number: 6  
Hi I'm the number: 7  
Hi I'm the number: 8  
Hi I'm the number: 9  
Hi I'm the number: 10
```

```
for i in {1..10}; do  
> echo "Iteraction $i";  
> done  
Iteraction 1  
Iteraction 2  
Iteraction 3  
Iteraction 4  
Iteraction 5  
Iteraction 6  
Iteraction 7  
Iteraction 8  
Iteraction 9  
Iteraction 10
```

## if
```
if true; then  
> echo "Ha ha"  
> fi  
Ha ha
```

### If existis a file
```
if [ -a hi ]; then  
> echo "Exist hi file";  
> fi  
Exist hi file
```

## Scripts

### Time is odd
```
if (( $(date +%s) % 2 )); then
  echo "is odd"
  exit 0
else
  echo "is even"
  exit 1
fi
```

### Time is odd with error
```
if (( $(date +%s) % 2 )); then
  echo "is odd"
  exit 0
else
  >&2 echo "is even"
  exit 1
fi
```

## Links
- [Bash scripting cheatsheet](https://devhints.io/bash)
- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html