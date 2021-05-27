# apertium-nio: Nganasan for apertium

Apertium monolingual analyser for Nganasan (latinised, based on one course work)

This is an Apertium monolingual language package for nio. What
you can use this language package for:

* Morphological analysis of nio
* Morphological generation of nio
* Part-of-speech tagging of nio

## Requirements

You will need the following software installed:

* lttoolbox (>= 3.3.0)
* apertium (>= 3.3.0)
* vislcg3 (>= 0.9.9.10297)
* hfst (>= 3.8.2)

If this does not make any sense, we recommend you look at: 
https://wiki.apertium.org

## Compiling

Given the requirements being installed, you should be able to just run:

    $ ./configure
    $ make

You need to use `./autogen.sh` before `./configure` if you're compiling
from git.

If you're doing development, you don't have to install the data, you
can use it directly from this directory.

If you are installing this language package as a prerequisite for an
Apertium translation pair, then do (typically as root / with sudo):

    # make install

You can give a --prefix to ./configure to install as a non-root user,
but make sure to use the same prefix when installing the translation
pair and any other language packages.

## Testing

If you are in the source directory after running make, the following
commands should work:

    $  echo "TODO: test sentence" | apertium -d . nio-morph
    TODO: test analysis result

    $ echo "TODO: test sentence" | apertium -d . nio-tagger
    TODO: test tagger result

##Files and data

* apertium-nio.nio.dix           - Monolingual dictionary
* apertium-nio.nio.lexc          - Morphotactic dictionary
* apertium-nio.nio.twol          - Morphophonological rules
* apertium-nio.nio.rlx           - Constraint Grammar disambiguation rules
* apertium-nio.post-nio.dix      - Post-generator
* nio.prob                       - Tagger model
* modes.xml                      - Translation modes

## For more information

* https://wiki.apertium.org/wiki/Installation
* https://wiki.apertium.org/wiki/apertium-nio
* https://wiki.apertium.org/wiki/Using_an_lttoolbox_dictionary

## Help and support

If you need help using this language pair or data, you can contact:

* Mailing list: apertium-stuff@lists.sourceforge.net
* IRC: #apertium on irc.oftc.net

See also the file AUTHORS included in this distribution.

