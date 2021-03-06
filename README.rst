.. image:: https://travis-ci.org/pebble/eachpng.svg?branch=master
    :target: https://travis-ci.org/pebble/eachpng
    
eachpng
=======

Executes command line for each PNG from standard input and forwards their output to stdout.
This can be useful if you are want to generate an animated GIF (e.g. which gifsicle) but
coming from a stream of of PNGs (which cannot be converted via ImageMagick as such).


Installing
----------

.. code-block:: bash
  
  $ pip install .


Running tests
-------------

.. code-block:: bash

  $ nosetests tests


Usage
-----

Here's an example how to convert a stream of PNGs to an animated GIF. This example uses:

- `seq` to create a sequence of numbers (stream: many lines with a number each line), uses
- `xargs` to call a fictitious program `./tool` that produces a PNG for each call (stream: sequence of PNGs)
- `eachpng` calling ImageMagick `convert` for each PNG to convert it to GIF (stream: sequence if GIFs)
- `gifsicle` operating taking the sequence of GIFs to produce an animated GIF


.. code-block:: bash

  $ seq 0 33 12000 | \
    xargs -L 1 -I TC ./tool -t TC -o - | \
    eachpng convert - GIF:- | \
    gifsicle --multifile --delay 3 -O3 >out.gif
