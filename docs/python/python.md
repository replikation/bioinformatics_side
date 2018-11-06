# Basics
* in shell do `python3` to open the command line
* `python3 skript.py` to execute a `.py` skript

## Installing pip
* due to the version chaos that is pip pip3 python and python3, here an explanation
* stick with python 3
* so if a site tells you to do `pip install` do `pip3 install`

```bash
# installing the pip3 from python3.X (not pip from python 2.7)
sudo apt-get install python3-pip ## use the apt-get for now
# example of a pip install, DONT use sudo here
pip3 install pylama pylama-pylint
```


## Special rules
* you can split a single line like this into multiple lines using `\`

```python
total = item_one + \
   item_two + \
   item_three
```

## Commands
```python
#!/usr/bin/python3
print("test") #basically like echo, includes \n

input("\n\nPress the enter key to exit.") #creates a user prompt, has two empty lines before

; # ";" is used like in shell to get more commands in one line
```

## Variables in python3
Usually
* Class names start with an uppercase letter
* All other identifiers start with a lowercase letter
* Starting an identifier with a single leading underscore indicates that the identifier is private
* don't use words like `for` etc for it
A few examples:

### Numeric Variables
```python
#!/usr/bin/python3

counter = 100           # An integer assignment
miles   = 1000.0        # A floating point
name    = "John"        # A string
cplx = 70.2-E12         # complex number
# also possible to add one input to multiple variables
a = b = c = 1
#or like an oneliner array (so a get 1 b 2 etc.)
a, b, c = 1, 2, "john"
# delete them with
del counter, miles
```
### String Variables

```python
#!/usr/bin/python3

str = 'Hello World!'

print (str)          # Prints complete string, starts at 0
print (str[0])       # Prints first character of the string
print (str[2:5])     # Prints characters starting from 3rd to 5th
print (str[2:])      # Prints string starting from 3rd character
print (str * 2)      # Prints string two times
print (str + "TEST") # works only on strings
```
### List Variables
* works a bit like in R
* can store different variable types, can be changed
* **a list works only with `[]`**
* a "read only" list is with`()`

```python
#!/usr/bin/python3

list = [ 'abcd', 786 , 2.23, 'john', 70.2 ]
tinylist = [123, 'john']

print (list)          # Prints complete list
print (list[0])       # Prints first element of the list
print (list[1:3])     # Prints elements starting from 2nd till 3rd
print (list[2:])      # Prints elements starting from 3rd element
print (tinylist * 2)  # Prints list two times
print (list + tinylist) # Prints concatenated lists


tuple = ( 'abcd', 786 , 2.23, 'john', 70.2 )
# print works exactly the same here
```

* there is also a dictionary type of list with `{}` its basically unordered, so you can shove data into it
  * check [here](https://www.tutorialspoint.com/python3/python_variable_types.htm)

## Loops
* are without braces, spacing and lines depends if it works
* examples:
```python
for i in [1,2,3]:
    print(i) #only shows the variable
    print('%sbp' % i) #this is how to put variables and text together
    print("____")
print("loop is ended, wow")

#first line with : is called header, all of it is called suite
if expression :
   suite
elif expression :
   suite
else :
   suite
```
