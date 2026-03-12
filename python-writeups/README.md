# Python Tasks

A collection of 4 Python programs written and run in Jupyter Notebook.

---

## Task 1 — XOR Pair Finder

Takes a list of numbers and a target value entered by the user. Finds and prints all pairs of numbers from the list whose XOR equals the target.

```python
def xor(l):
    l2=[]
    target=int(input("enter the target:"))
    for i in l:
        for j in l:
            if i^j==target:
                l2.append((i,j))
    print(l2)
xor([1,2,3,4,5,6])
```

**Output:**
```
enter the target: 3
[(1, 2), (2, 1), (2, 3), (3, 2), (5, 6), (6, 5)]
```

---

## Task 2 — Palindrome Checker

Takes a string entered by the user and checks whether it reads the same forwards and backwards. Prints whether it is a palindrome or not.

```python
def ispalindrome(s):
    rev=(s[::-1])
    if  rev==s:
        return 0
    return 1
stri=input("enter the string")
pal=ispalindrome(stri)
if pal==0:
    print(stri,'is palindrome')
else:
    print(stri,'not ispalindrome')
```

**Output:**
```
enter the string: racecar
racecar is palindrome
```
```
enter the string: hello
hello not ispalindrome
```

---

## Task 3 — Anagram Detector

Takes two strings and checks if they are anagrams of each other — same letters, regardless of order or case. Prints True or False.

```python
def angrams(str1,str2):
    return print (sorted(str1.lower())==sorted (str2.lower()))
angrams("hello","HELLO")
```

**Output:**
```
True
```

---

## Task 4 — Reverse Only Alphanumeric Characters

Takes a string with letters, numbers, and special characters. Reverses only the alphanumeric characters while keeping all special characters fixed in their original positions.

```python
def revlet(text):
    letters = []
    for ch in text:
        if ch.isalnum():
            letters.append(ch)
    result = ""
    for ch in text:
        if ch.isalnum():
            result += letters.pop()
        else:
            result += ch
    return result
print(revlet("a2$mrita"))
```

**Output:**
```
atirm2$a
```

---

## Author

**[Your Name]**  
GitHub: [github.com/your-username](https://github.com/your-username)
