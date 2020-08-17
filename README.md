# Fuzzy-Search

import string
import time

count = 0 
def main():
    fileName = input("Please input file name ")
    word = input("Please input the word you so desire to find: ")

    fileText = read_file(fileName)
    #print(fileText)

    startTime = time.time()

    amounfOfShifts = shiftTable(word)
    print(amounfOfShifts)                  

    q =search(fileText,word,amounfOfShifts)
    if q == -1:
        print("No Exact Match")
    else:
        print("There is an exact word: ")
        print(q, "-" , (len(word) - 1) + q)


    print("Swaps: ")
    s = swap(word)
    swappy = []
    for line in range(len(s)):
        shifts = shiftTable(s[line])
        swappy = search(fileText,s[line],shifts)
        if swappy != -1:
            print(swappy, "-" , (len(s[line]) - 1) + swappy)

    print("Deletions: ")
    d = deletion(word)
    delety = []
    for index in range(len(d)):
        ggg = shiftTable(d[index])
        delety = search(fileText,d[index],ggg)
        if delety != -1:
            print(delety, "-", ((len(d[index]) - 1) + delety))

    print("Substitution: ")
    sub = substitution(word)
    subby = []
    for index2 in range(len(sub)):
        hh = shiftTable(sub[index2])
        subby = search(fileText,sub[index2],hh)
        if subby != -1:
            print(subby, "-", ((len(sub[index2]) -1) + subby))


    print("Insertion: ")
    ins = insert(word)
    insty = []
    for every in range(len(ins)):
        vv = shiftTable(ins[every])
        insty = search(fileText,ins[every],vv)
        if insty != -1:
            print(insty, "-", ((len(ins[every])) + insty))

    endTime = time.time()

    print("The amount of time it takes is: ",endTime - startTime)


    print("Counts: ", count)






def returnnumShift(shifts,letter,word):         
    if letter in shifts:
        return shifts.get(letter)
    else:
        return len(word)



def search(fileText, word,amountOfShifts):
    global count
    i = len(word) -1
    while (i <= len(fileText) -1):
        k = 0

        while (k <= len(word) - 1 and word[len(word)- 1- k] == fileText[i-k]):
            count = count + 1
            k = k + 1
        if(k == len(word)):
            return i - len(word) + 1
        else:
            i = i + returnnumShift(amountOfShifts,fileText[i],word)
    return -1


def swap(word):
    s = []
    for index in range(len(word)-1):
        s.append(''.join((word[:index], word[index + 1], word[index + 1:index + 1], word[index], word[index + 1 + 1:])))

    return s


def read_file(filename):
    words = []
    bob = open(filename, "r")
    text = bob.read() 

    return text


def shiftTable(word):
    lenght = len(word)
    cast = {}
    value = list(word)
    for index in range(len(word)):
        #print(word[index])

        if(len(word) - 1 == index):
            maybe = checkKey(cast, word[index])
            if(not maybe):
                cast[word[index]] = len(word)
        else:
            cast[word[index]] = len(word) - index - 1
    return cast


def checkKey(dict,key):
    if key in dict.keys():
        return True
    else:
        return False

def deletion(word):
    newWord = []
    for index in range(len(word) + 1):
        var = (word[0: index - 1] + word[index:len(word)])
        newWord.append(var)
    del(newWord[0])
    return newWord

def substitution(word):
    var = list(word)
    newList = []
    counter = 0
    alpha = string.ascii_lowercase
    for index in range(len(word)):
        for char in alpha:
            newWord = list(word)
            if(newWord[index] != char):
                newWord[index] = char
                ''.join(newWord)
                newList.append(newWord)
                counter = counter + 1
    return newList

def insert(word):
    insertList = []
    alpha = string.ascii_lowercase
    for p in range(len(word) + 1):
        for c in range(len(alpha)):
            newWord = word[:p] + alpha[c] + word[p:]
            insertList.append(newWord)
    return insertList



if __name__ == "__main__":
    main()
