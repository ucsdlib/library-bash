---
title: "Counting and mining with the shell"
teaching: 40
exercises: 20
questions:
- "How do you find data within files?"
- "How do you count data?"
objectives:
- "understand how to count lines, words, and characters with the shell"
- "understand how to mine files and extract matched lines with the shell"
- "understand how to combine mining with the shell and regular expressions"
keypoints:
- "wc is a command that counts"
- "grep is command that searches inside texts"
---
##  Manipulating, counting and mining research data

Now you can work with the unix shell you can move onto learning how to count and mine data. 
These are rather simple and are unlikely to totally revolutionise your work. 
They are, however, alongside the consistent file structure and naming I touched on last week, 
the foundation of a more powerful set of commands that can count and mine your data.

## Counting

You will begin by counting the contents of files using the Unix shell. 
The Unix shell can be used to quickly generate counts from across files, 
something that is tricky to achieve using the graphical user interfaces of standard office suites.

In the Unix shell, use the `cd` command to navigate to the directory 
that contains our data. 

~~~
cd data
~~~
{: .bash}


Remember, if at any time you are not sure where you are in your directory structure, type `pwd` and hit enter.

~~~
pwd
~~~
{: .bash}

~~~
/Users/riley/Desktop/data
~~~
{: .output}

Type `ls -lh` and then hit enter. This prints, or displays, a list that includes a file and a subdirectory.

~~~
ls -lh
~~~
{: .bash}

~~~
total 90120
-rwxr-xr-x  1 riley  staff   3.6M Jul 17 14:33 2014-01-31_JA-africa.tsv
-rwxr-xr-x  1 riley  staff   7.4M Jul 17 14:33 2014-01-31_JA-america.tsv
-rw-r--r--  1 riley  staff    29M Jul 17 14:33 2014-01_JA.tsv.zip
-rwxr-xr-x  1 riley  staff   1.8M Jul 17 14:33 2014-02-01_JA-art .tsv
-rwxr-xr-x  1 riley  staff   1.4M Jul 17 14:33 2014-02-02_JA-britain.tsv
-rwxr-xr-x  1 riley  staff    13B Jul 17 14:33 gallic.txt
-rwxr-xr-x  1 riley  staff   598K Jul 17 14:33 gulliver.txt
~~~
{: .output}

The data directory contains a zipped up version of the dataset `2014-01_JA.tsv` that contains journal article metadata, four .tsv files derived from `2014-01_JA.tsv`, the gulliver.txt file we created in episode 1, and a gallic.txt file we'll talk about later. Each of these four .tsv files includes all data where a keyword such as `africa` or `america` 
appears in the 'Title' field of `2014-01_JA.tsv`. Before we start, please unzip the `2014-01_JA.tsv.zip` file into it's containing directory (`data/`). 

> ## TSV Files
>TSV files are those in which within each row the units of data 
>(or cells) are separated by tabs. They are similar to CSV (comma separated value) 
>files were the values are separated by commas. The latter are more common
>but can cause problems with the kind of data we have, where commas can be 
>found within the cells (though with the right encoding this can be overcome). 
>Either way both can be read in simple text editors or in spreadsheet programs 
>such as Libre Office Calc or Microsoft Excel.*
{: .callout}

From within the `data/` directory, we can count the contents of the files.

The Unix command for counting is `wc`. The flag `-w` combined with `wc` instructs the computer to print a word count, and the name of the file that has been counted, into the shell.

~~~~
wc -w 2014-01-31_JA-africa.tsv 
~~~~
{: .bash}

~~~
511261 2014-01-31_JA-africa.tsv
~~~
{: .output}

As was seen earlier today flags such as `-w` are an essential part of getting 
the most out of the Unix shell as they give you better control over commands.

If your reader request or piece of work is more concerned number of entries (or lines) 
than the number of words, you can use the line count flag. 

~~~
wc -l 2014-01-31_JA-africa.tsv
~~~
{: .bash}

~~~
13712 2014-01-31_JA-africa.tsv
~~~
{: .output}

Combined with `wc` the flag `-l` prints a line count and the name of the file that has been counted.

Finally, type

~~~
wc -c 2014-01-31_JA-africa.tsv 
~~~
{: .bash}

~~~
3773660 2014-01-31_JA-africa.tsv
~~~
{: .output}

and hit enter. This uses  the flag `-c` in combination with the command `wc` to print a character 
count for `2014-01-31_JA-africa.tsv` 

**Note: OS X users should replace the -c flag with -m.** 

With these three flags, the most obvious thing we can use `wc` for is to 
quickly compare the shape of sources in digital format - for example word 
counts per page of a book, the distribution of characters per page across 
a collection of newspapers, the average line lengths used by poets. 
You can also use `wc` with a combination of wildcards and flags to build more complex queries.

> ## WC on Multiple Files
>Can you guess what the line `wc -l 2014-01-31_JA-a*.tsv` will do? 
>
>~~~
>wc -l 2014-01-31_JA-a*.tsv
>~~~
>{: .bash}
>> ## Solution
>>~~~
>>13712 2014-01-31_JA-africa.tsv
>>27392 2014-01-31_JA-america.tsv
>>41104 total
>>~~~
>>This prints the line counts for `2014-01-31_JA-africa.tsv` 
>>and `2014-01-31_JA-america.tsv`, offering a simple means of comparing 
>>these two sets of research data.
>{: .solution}
{: .output}

Of course, it may be faster if you 
only have a handful of files to compare the line count for the two 
documents in Libre Office Calc, Microsoft Excel, or a similar spreadsheet 
program. But when wishing to compare the line count for tens, hundreds, or 
thousands of documents, the Unix shell has a clear speed advantage.

Moreover, as our datasets increase in size you can use the Unix 
shell to do more than copy these line counts by hand, by the use of 
print screen, or by copy and paste methods. Using the `>` redirect 
operator we saw earlier you can export our query results to a new file. 

~~~
wc -l 2014-01-31_JA-a*.tsv > results/2016-07-19_JA-a-wc.txt
~~~
{: .bash}

~~~
-bash: results/2016-07-19_JA-a-wc.txt: No such file or directory
~~~
{: .error}

Here we've received a bash error message letting us know that there isn't a `results/` directory to save our new file in. Let's remedy this by creating that directory. 

~~~
mkdir results
~~~
{: .bash}

~~~
ls -F
~~~
{: .bash}

~~~
2014-01-31_JA-africa.tsv*	2014-02-02_JA-britain.tsv*
2014-01-31_JA-america.tsv*	gallic.txt*
2014-01_JA.tsv			gulliver.txt*
2014-01_JA.tsv.zip		results/
2014-02-01_JA-art .tsv*
~~~
{: .output}

Ok, once the `results/` directory exists, we can try our `wc` to a file command again. 

~~~
wc -l 2014-01-31_JA-a*.tsv > results/2016-07-19_JA-a-wc.txt
~~~
{: .bash}

This runs the same query as before, but rather than print the 
results within the Unix shell it saves the results as `DATE_JA_a-wc.txt`. 
By prefacing this with `results/` the shell is instructed to save the .txt 
file to the `results` sub-directory. To check this, navigate to the `results` 
subdirectory:  

~~~
cd results
ls 
~~~
{: .bash}

~~~
2016-07-19_JA-a-wc.txt
~~~
{: . output}

To see the file contents in the shell (as it 
is 10 lines or fewer in length, all the file contents will be shown here): 

~~~
head 2016-07-19_JA-a-wc.txt
~~~
{: .bash}

~~~
   13712 2014-01-31_JA-africa.tsv
   27392 2014-01-31_JA-america.tsv
   41104 total
~~~
{: .output}

## Mining

The Unix shell can do much more than count the words, characters, and lines within a file. 
The `grep` command (meaning **global regular expression print**) is 
used to search across multiple files for specific strings of characters. 
It is able to do so much faster than the graphical search interface 
offered by most operating systems or office suites. And combined with the `>` 
operator, the `grep` command becomes a powerful research tool can be used 
to mine your data for characteristics or word clusters that appear across 
multiple files and then export that data to a new file. The only limitations 
here are your imagination, the shape of your data, and - when working with 
thousands or millions of files - the processing power at your disposal.

To begin using `grep`, first navigate to the `data` directory (from results/ type `cd ..`). 

~~~
grep 1999 *.tsv
~~~
{: .bash}

This query looks across all files in  the directory that fit the given criteria (the .tsv files) for instances of the string, or character cluster, '1999'. It then prints them within the shell.

Press the up arrow once in order to cycle back to your most recent action. 
Amend `grep 1999 *.tsv` to `grep -c 1999 *.tsv` and hit enter. 

~~~
grep -c 1999 *.tsv
~~~
{: .bash}

~~~
2014-01-31_JA-africa.tsv:804
2014-01-31_JA-america.tsv:1478
2014-01_JA.tsv:28767
2014-02-01_JA-art .tsv:407
2014-02-02_JA-britain.tsv:284
~~~
{: .output}

The shell now prints the number of times the string 1999 appeared in each `*.tsv file`. 
If you look at the output from the previous command, this tends to be refer to the 
date field for each journal article.

Strings need not be numbers. 

~~~
grep -c revolution *.tsv
~~~
{: .bash}


~~~
2014-01-31_JA-africa.tsv:20
2014-01-31_JA-america.tsv:34
2014-01_JA.tsv:867
2014-02-01_JA-art .tsv:11
2014-02-02_JA-britain.tsv:9
~~~
{: .output}

Counts 
the instances of the string `revolution` within the defined files and prints 
those counts to the shell. Now, amend the above command to the below and observer how the outpu of each is different: 

~~~
grep -ci revolution *.tsv
~~~
{: .bash}

~~~
2014-01-31_JA-africa.tsv:118
2014-01-31_JA-america.tsv:1018
2014-01_JA.tsv:9327
2014-02-01_JA-art .tsv:110
2014-02-02_JA-britain.tsv:122
~~~
{: .output}

This repeats the query, but prints a case 
insensitive count (including instances of both `revolution` and `Revolution`). 
Note how the count has increased nearly 30 fold for those journal article 
titles that contain the keyword 'america'. As before, cycling back and 
adding `> results/`, followed by a filename (ideally in .txt format), will save the results to a data file.

So far we have counted strings in file and printed to the shell or to 
file those counts. But the real power of `grep` comes in that you can 
also use it to create subsets of tabulated data (or indeed any data) 
from one or multiple files.  

~~~
grep -i revolution *.tsv
~~~
{: .bash}

This script looks in the defined files and prints any lines containing `revolution` 
(without regard to case) to the shell. 

~~~
grep -i revolution *.tsv > results/2016-07-19_JAi-revolution.tsv
~~~
{: .bash}

This saves the subsetted data to file.

However if we look at this file, it contains every instance of the 
string 'revolution' including as a single word and as part of other words 
such as 'revolutionary'. This perhaps isn't as useful as we thought... 
Thankfully, the `-w` flag instructs `grep` to look for whole words only, 
giving us greater precision in our search. 

~~~
grep -iw revolution *.tsv > results/DATE_JAiw-revolution.tsv
~~~
{: .bash} 

This script looks in both of the defined files and
exports any lines containing the whole word `revolution` (without regard to case) 
to the specified .tsv file. 

We can show the difference between the files we created.

~~~
wc -l results/*.tsv
~~~
{: .bash}

~~~
   10695 2016-07-19_JAi-revolution.tsv
    7859 2016-07-19_JAw-revolution.tsv
   18554 total
~~~
{: .output}


Finally, you can use the **regular expression syntax** covered earlier to search for similar words. 

In `gallic.txt` we have the string `fr[ae]nc[eh]`. 

~~~
cat gallic.txt
~~~
{: .bash}

~~~
fr[ae]nc[eh]
~~~
{: .output}

The square brackets here ask the machine to match any character 
in the range specified. So when used with grep ...

~~~
grep -iw --file=gallic.txt *.tsv
~~~
{: .bash}

... the shell will print out each line containing the string:

~~~
- france
- french
- frence
- franch
~~~
{: .output}

Include the `-o` flag to print only the matching part of the lines e.g. (handy for isolating/checking results).

~~~
grep -iwo revolution *.tsv
~~~
{: .bash}

OR: 

~~~
grep -iwo --file=gallic.txt *.tsv
~~~
{: .bash}

Pair up with your neighbor and work on these exercies: 

>## Case sensitive search
>Search for all case sensitive instances of 
>a word you choose in all four derived tsv files in this directory. 
>Print your results to the shell.
>
>>## Solution
>>~~~
>>grep hero *.tsv`
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

> ## Case sensitive search 2
>Search for all case sensitive instances of a word you choose in 
>the 'America' and 'Africa' tsv files in this directory. 
>Print your results to the shell.
>
>>## Solution
>>~~~
>>grep hero 2014-01-31*`
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

>## Case sensitive search 3
>Count all case sensitive instances of a word you choose in 
>the 'America' and 'Africa' tsv files in this directory. 
>Print your results to the shell.
>
>># Solution
>>~~~
>>grep -c hero 2014-01-31*
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

>## Case insensitive search 
>Count all case insensitive instances of that word in the 'America' and 'Africa' tsv files 
>in this directory. Print your results to the shell.
>
>>##Solution
>>~~~
>>grep -ci hero 2014-01-31*`
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

>## Case insensitive search 2 
>Search for all case insensitive instances of that 
>word in the 'America' and 'Africa' tsv files in this directory. Print your results to a new >.tsv file. 
>
>> ## Solution
>>~~~
>>grep -i hero 2014-01-31* > new.tsv
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

>## Case insensitive search 3
>Search for all case insensitive instances of that whole word 
>in the 'America' and 'Africa' tsv files in this directory. Print your results to a new .tsv    >file.
>> ## Solution
>>~~~
>>grep -iw hero 2014-01-31* > new2.tsv
>>~~~
>>{: .bash}
>{: .solution}
{: .challenge}

Compare the line counts of the last two files you created.

~~~
wc -l FILENAMES
~~~
{: .bash}

Open both files in a text editor (Notepad++, Atom, Kate, 
whatever you prefer) or Excel-like program to see the difference 
between searching strings and searching whole words using `grep`

## Recap

Within the Unix shell you can now:

- use the `wc` command with the flags `-w` and `-l` to count the words and lines in a file or a series of files.
- use the redirector and structure `> subdirectory/filename` to save results into a subdirectory.
- use the `grep` command to search for instances of a string.
- use with `grep` the `-c` flag to count instances of a string, the `-i` flag to return a case insensitive search for a string, the `-v` flag to exclude a string from the results, and -w to return a whole word only search
- use - `--file=list.txt` to use the file `list.txt` as the source of strings used in a query
- combine these commands and flags to build complex queries in a way that suggests the potential for using the Unix shell to count and mine your research data and research projects.

**DATA CAPTURE**

At this point, I'd like you to describe on your sticky note, briefly, a use case for this that might change your practice.
