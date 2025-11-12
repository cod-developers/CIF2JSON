CIF <-> JSON
============

We at the
[Crystallography Open Database](http://www.crystallography.net) (COD)
have been experimenting
with CIF <-> JSON conversion for a while. We have arrived at a decision
to map the internal representation of CIF files used by our software
package [cod-tools](http://wiki.crystallography.net/cod-tools/)
directly into JSON. We have been using 'cod-tools' internal
representation (described in
[Merkys et al., 2016](http://dx.doi.org/10.1107/S1600576715022396))
for some time and though it includes some redundant information, it
has proven itself useful in automatically handling CIFs at the COD.
Another,
[low redundancy method](https://github.com/COMCIFS/comcifs.github.io/blob/master/cif-json.md)
was also proposed.

Main priciples of CIF -> JSON conversion
----------------------------------------

1. Top level container is an array instead of an object, thus, order
   of CIF datablocks (represented as objects) in an input file is
   retained; uniqueness of datablock names are not enforced;

2. Data items are keys of ``values`` sub-object, thus uniqueness of
   data names within a data block / save frame are enforced;

3. Values are *always* represented as strings exactly as given in CIF
   file, without losing any precisions;

3. Types of values are stored alongside in a sub-object ``types`` of
   datablock. Thus, ``?`` value of type 'unquoted string' is easily
   distinguishable from ``?`` value of type 'quoted string' or
   'textfield', reducing the need of methods to escape ``?`` and ``.``
   values with special meanings;

4. Values of a tag are always put in an array; there is a sub-object
   ``inloop`` of datablock, which tells whether a tag is looped or
   not;

5. Loops, their tags and order are described in sub-array ``loops``
   of datablock.

Example
-------

Converts CIF file ([test.cif](test.cif)) to JSON ([test.json](test.json)):

    $ cif2json test.cif > test.json

Pretty-prints converted JSON to [test-pp.cif](test-pp.cif):

    $ json_pp < test.json > test-pp.json
