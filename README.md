# Steganography
Python Code to Steganize your Tweets
from nltk import word_tokenize
from nltk.util import ngrams
from collections import Counter
import nltk.data 

# nltk.download("punkt")

import atexit
from time import time, strftime, localtime
from datetime import timedelta

def secondsToStr(elapsed=None):
    if elapsed is None:
        return strftime("%Y-%m-%d %H:%M:%S", localtime())
    else:
        return str(timedelta(seconds=elapsed))

def log(s, elapsed=None):
    line = "="*40
    print(line)
    print(secondsToStr(), '-', s)
    if elapsed:
        print("Elapsed time:", elapsed)
    print(line)
    print()

def endlog():
    end = time()
    elapsed = end-start
    log("End Program", secondsToStr(elapsed))

start = time()
atexit.register(endlog)
log("Start Program")



 
filein = '/Users/Honeywell/Downloads/testout3.txt'
# fileout = '/Users/rajrajagopalan/Downloads/unigrams.txt'
g = open(filein)
#h = open(fileout)
text = g.read()
token = nltk.word_tokenize(text)
#    "unigrams = ngrams(token,1)\n",
#    "print(Counter(unigrams)) >> \"unigrams.txt\"\n",
bigrams = ngrams(token,2)
print(bigrams)
#print(Counter(bigrams))
#trigrams = ngrams(token,3)\n",
#print(Counter(trigrams))\n",
#fourgrams = ngrams(token,4)\n",
#print(Counter(fourgrams))\n",
g.close()
    
    
from numpy import random
import nltk
from nltk import word_tokenize
from nltk.util import ngrams
from collections import Counter
import re
from nltk.tokenize.treebank import TreebankWordDetokenizer as Detok

# codewords = ["mango", "cheese", "apple", "pasta", "flour"]

codewords = ["really", "morning", "week", "good", "long", "tomorrow", "well", "school", "tired", "weekend"]
# File with tweets
fname = "/Users/Honeywell/testout3.txt"
line = []
# pick a good tweet
print("starting")

#  check line is kosher

def testingTweet (testline) :
    linewords = nltk.word_tokenize(testline)            

    if len(linewords) < 4:
        return 0;
    
    for word in linewords:
        if word in codewords:
              return 0;

    return 1;


def pickTweetNoTest ():
    r = random.randint(0,240297)

    with open(fname, 'r') as f:
        count = 0

        for line in f:
            if count == r:
# line is the random tweet
                f.close()
                return line
            else:
                count = count + 1
        
    
    
def createStego (line, codeword1, codeword2) :

    length = len(line.split())
    newStego = []
#pick two random positions in the tweet
            
    pos1 = random.randint(0,length-1)
    pos2 = random.randint(pos1+1, length)
#            print(pos)
    count2 = 0;
    
    linewords = nltk.word_tokenize(line)
    for word in linewords:
        if (count2 == pos1):
            newStego.append(codeword1)
        if (count2 == pos2):
            newStego.append(codeword2)
            print(word)     
        newStego.append(word)
        count2 = count2 + 1
    
# corner case when pos2 is at the end of the sentence, then count2 would have caught up to pos2
    
    if (count2 == pos2):
        newStego.append(codeword2)
    return newStego

message1 = random.randint(0,9)
message2 = random.randint(0,9)
print(message1)
print(message2)

#pick the codewords corresponding to the messages
codeword1 = codewords[message1]
codeword2 = codewords[message2]

def decodeStego(line) :
    for word in line:
        if word in codewords:
# collision
            if (codewords.index(word) != message1) and (codewords.index(word) != message2) :
                return 0;
            else :
                print(codewords.index(word))


supercount2 = 0;
done = 0;
while (supercount2 < 100) and (done == 0) :

#    line = pickTweet()
    line = pickTweetNoTest()
    if line is None :
        print("Skipping stego step")
    else :
        print("Tweet picked:", line)

        stegoTweet = createStego(line, codeword1, codeword2)


#reconstruct the Tweet 
        detokenizer = Detok()
        newTweet = detokenizer.detokenize(stegoTweet)
        print("Stego: ", newTweet)

        if decodeStego(stegoTweet) == 0:
            print("Collision")


#Test detectability using bigrams of the new tweet

        newbigrams = ngrams(stegoTweet,2)
        print(newbigrams)
        done = 1
        for gram in newbigrams:
            if (gram in bigrams):
                print("good Bigram",  gram)
            else:
                print("bad Bigram", gram)
#                done = 0
        supercount2 = supercount2 + 1
    
# print(Counter(newbig

#dec
