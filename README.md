# py-rstr-max
Automatically exported from code.google.com/p/py-rstr-max

py-rstr-max : detection of all maximal repeats in strings, a python implementation
What does it look for?
Usage
Bench py-rstr-max please
Have fun with py-rstr-max
py-rstr-max formally
See also
What does it look for?
This implementation allows the detection, in linear time, of all the maximal repeats in one, or more, strings.

The complete extraction is done in quasi-linear time |n + z| where z is the number of maximal repeats in S. This implementation uses the computation of suffix array in linear time implemented in the pysuffix project

The longest common prefix computation used is described in this paper http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.118.8221 and is included in the last version of pysuffix project

Maximal repeats are also called non-gapped motif (http://www.springerlink.com/content/7215807755758m35/ Structural Analysis of Gapped Motifs of a String, Esko Ukkonen, 2007).

Usage
Just go to the svn version ; or download the last version http://code.google.com/p/py-rstr-max/downloads/list

user@machine: tar -xvzf py-rstr-max.tar.gz
user@machine: cd py-rstr-max
user@machine: python rstr_max.test.py
Inside rstr_max.test.py (4.0)
Feed the process, here the string 'tititoto'

#!/usr/bin/env python
# -*- coding: utf-8 -*-

from tools_karkkainen_sanders import *
from rstr_max import *

str1 = "tototiti"

str1_unicode = unicode(str1,'utf-8','replace')

rstr = Rstr_max()
rstr.add_str(str1_unicode) #str1
r = rstr.go()
Explore the results :

for (offset_end, nb), (l, start_plage) in r.iteritems():
  ss = rstr.global_suffix[offset_end-l:offset_end]
  id_chaine = rstr.idxString[offset_end-1]
  s = rstr.array_str[id_chaine]
  print '[%s] %d'%(ss.encode('utf-8'), nb)
  for o in range(start_plage, start_plage + nb) :
    offset_global = rstr.res[o]
    offset = rstr.idxPos[offset_global]
    id_str = rstr.idxString[offset_global]
    print '   (%i, %i)'%(offset, id_str)
Will print :

[t] 4
   (6, 0)
   (4, 0)
   (2, 0)
   (0, 0)
[tot] 2
   (2, 0)
   (0, 0)
[ti] 2
   (6, 0)
   (4, 0)
Bench py-rstr-max please
Tests are done with a 2.4 Ghz core. Please, use PyPy.

bench_rstr
bench_rstr_pypy
cPython	PyPy 1.6
Have fun with py-rstr-max
If you feel the fun of the Zipf's law, you will appreciate rstr_max.zipf.py. This script show you how simple is the computation of the rank / freq. ratio (see bellow with log. scale).

r = rstr.go()
l = r.keys()

def cmpval(x,y):
  return y[1] - x[1]

l.sort(cmpval)
list_x = [math.log(elt[1]) for elt in l]
list_y = [math.log(i) for i in range(1, len(list_x)+1)]

plt.plot(list_x, list_y, 'r-')
filename = 'zipf_rstr.png'
plt.xlabel('log(rank)')
plt.ylabel('log(freq.)')
plt.savefig(filename)
bench_rstr
py-rstr-max formally
Let S a string of lenght ''n'' over a finite alphabet Σ.
Si refer to the i-th character of S.
Si..j refers to a substring of S starting at the position i and ending at position j.
Each position 0 ≤ i < n represents a unique suffix Si..n-1 of S.
LCi refers to the Left Context of a suffix i. We note £ the special left context LC0.
We note with the special symbol $ the character Sn.
A triple (p1 , p2 , l) is called repeat in S iff :

(p1+l-1 < n) ∧ (p2+l-1 < n) ∧ p1 ≠ p2
Sp1..(p2+l-1) = Sp2..(p2+l-1)
A repeat is called Left Maximal if LCp1 ≠ LCp1 ∨ LCp1 = £ ;
A repeat is called Right Maximal if Sp1..(p1+l) ≠ Sp2..(p2+l) ∨ Sp1..(p1+l) = $ ;
A repeat is Maximal if it is Left Maximal and Right Maximal .
This implementation extracts all the Maximal repeats in a string S. This is allowed by :

computing the suffix array of S
computing the longest common prefix of each suffix (ie: computing the augmented suffix array)
filtering each suffixes strictly included in an other.
See also
pysuffix project
phpsuffix project
php-rstr-max project
And also
http://pyhtmllist.sourceforge.net/ : pyhtmllist project
http://pypy.org/download.html : PyPy project
