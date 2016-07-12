
# Introduction

In this session we will introduce programming by looking at how data can be manipulated, counted, and mined using the Unix shell, a command line interface to your computer and the files to which it has access.

A Unix shell is a command-line interpreter that provides a user interface for the Linux operating system and for Unix-like systems (such as iOS). For Windows users, popular shells such as Cygwin or Git Bash provide a Unix-like interface (a command line interface preferable - to me at least - to Windows own flavour of command line). This session will cover a small number of basic commands using Git Bash for Windows users, Terminal for iOS. These commands constitute building blocks upon which more complex commands can be constructed to fit your data or project.

The motivations for wanting to learn shell commands are many and various. What you can quickly learn is how to query lots of data for the information you want super fast. Say, for example, you have a query from a reader about the number of articles published in 2009 in academic history journals whose title contains the word ‘International’. Now you could search a database. Alternatively you could work directly with the relevant data. The British Library has open data on journal articles, and I’ve prepared from that…


```bash
wc -l 2014-01_JA.tsv
```

      507732 2014-01_JA.tsv


… a 500,000 line data file containing that information. Excel will struggle to manipulate that, but the shell won’t. Let’s look at this shell command:


```bash
grep 2009 2014-01_JA.tsv | grep INTERNATIONAL | awk -F'\t' '{print $5}' | sort | uniq -c

```

      40 AFRICA -LONDON- INTERNATIONAL AFRICAN INSTITUTE-
       1 ARCHAEOLOGY ETHNOLOGY AND ANTHROPOLOGY OF EURASIA
      34 AUSTRALIAN JOURNAL OF INTERNATIONAL AFFAIRS
     947 BAR INTERNATIONAL SERIES
     105 CHINA REVIEW INTERNATIONAL
      70 FOREIGN POLICY -WASHINGTON-
      13 GESTA
      38 INDONESIAN QUARTERLY
       2 INTERNATIONAL AFRICAN LIBRARY
     274 INTERNATIONAL HISTORY REVIEW
      80 INTERNATIONAL JOURNAL OF AFRICAN HISTORICAL STUDIES
      17 INTERNATIONAL JOURNAL OF AFRICAN RENAISSANCE STUDIES
      23 INTERNATIONAL JOURNAL OF CONTEMPORARY IRAQI STUDIES
      16 INTERNATIONAL JOURNAL OF HISTORICAL ARCHAEOLOGY
      13 INTERNATIONAL JOURNAL OF IBERIAN STUDIES
     217 INTERNATIONAL JOURNAL OF MARITIME HISTORY
     169 INTERNATIONAL JOURNAL OF MIDDLE EAST STUDIES
      95 INTERNATIONAL JOURNAL OF NAUTICAL ARCHAEOLOGY
      70 INTERNATIONAL JOURNAL OF OSTEOARCHAEOLOGY
      25 INTERNATIONAL JOURNAL OF REGIONAL AND LOCAL STUDIES
      55 INTERNATIONAL JOURNAL OF THE CLASSICAL TRADITION
      46 INTERNATIONAL REVIEW OF SOCIAL HISTORY
      42 INTERNATIONALES ASIENFORUM
      28 JEUNE AFRIQUE
       1 JOURNAL OF AFRICAN AMERICAN HISTORY
      31 JOURNAL OF CONTEMPORARY AFRICAN STUDIES
       1 JOURNAL OF MODERN AFRICAN STUDIES
       2 RESEARCH IN INTERNATIONAL STUDIES SOUTHEAST ASIA SERIES
       1 REVOLUTIONARY RUSSIA


This is simple, powerful, and does what we want. It may seem intimidating but, as you’ll discover this evening, it is deeply logical and eminently within your reach. Let us go through each part in turn:

This is simple, powerful, and does what we want. 
It may seem intimidating but, as you'll discover this evening, 
it is deeply logical and eminently within your reach. Let us go through each part in turn:

- `grep 2009 2014-01_JA.tsv` : this tells the machine to look in the spreadsheet 2014-01_JA.tsv for all the lines that contain the string 2009 and to store those in memory. The pipe then tells the machine to hold those in memory for the minute as we have something else we want to do...
- `grep INTERNATIONAL` : ...that is look for the capitalised string `international` on those lines that have 2009 in them. The shell is case sensitive by default and I know that in my data most (if not all) occurrences of international in caps will be in a column that lists journal titles. Note: this isn't super exact, but I know my data and know it'll do the job of now (think back to the automate vs manual discussion last week). Again it holds this subset in memory and...
- `awk -F'\t' '{print $5}'` : ...moves on to the next bit. This is the most fiddly of the bits and not something we will cover properly today. But all it says is that 2014-01_JA.tsv is a tab separated spreadsheet and to print out to the shell the 5th column (which is the one I know contains journal titles) of all the lines we've queried down to (those with 2009 in them, and then those with INTERNATIONAL in them) and to hold that in memory so that we can then...
- `sort` : ...sort that column. `sort` does what is says on the tin, and sorts the data we have left (which should just a single column containing journal titles) holding that sorted list in memory...
- `uniq -c` : ... so that we can then, finally, tell the machine to remove duplicates but as it is doing so count those duplicates and hold that data in memory.

As this is the last bit, the shell then - by default - prints the results to a shell: 
the number of articles published in 2009 in academic journals whose title contains the word 'International', 
with counts separated by journal. This last bit I have added in as, as we can see, 
we have a few false positives. We'd have to go back to our data to find out why, but this is a very good start: 
from 500,000 lines of journal article metadata to a few numbers and names in a small line of code.

You won't be doing exactly this by the end, but you won't be far off. 
We'll be covering quite a lot but the key commands are in the handout for reference.
We'll proceed in a follow my leader fashion. The format will be that I'll demo a command, you copy, 
and then we'll discuss the results. Stickies for when you get stuck or something doesn't appear to work.

## Basics - navigating the shell

We will begin with the basics of navigating the Unix shell.

Start by opening your shell. When you run it, you will likely see a black window with a cursor flashing next to a dollar sign. 
This is our command line, and the `$` is the command **prompt** to show the system is ready for your input.

If, when opening a command window or at any other time today, 
you are unsure of where you are in a computer's file system, 
you can find out what directory you are in using `pwd` command, 
which stands for "print working directory", and hitting enter - which executes commands in the shell. 
Try typing `pwd` and hitting enter.


```bash
pwd
```

    /Users/jtdennis/workshops/library-shell/data



```bash

```
