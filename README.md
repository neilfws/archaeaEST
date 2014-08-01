# README

## Introduction

This repository contains code and data for discovering transcripts in the human subset of the [EST database](http://www.ncbi.nlm.nih.gov/nucest) that may originate from human-associated Archaea.

It is my attempt to make amends for the publication [An online database for the detection of novel archaeal sequences in human ESTs](http://bioinformatics.oxfordjournals.org/content/20/15/2361.abstract), which became a [404 not found](http://bioinformatics.oxfordjournals.org/content/24/11/1381.full) long ago.

See [my blog post](http://nsaunders.wordpress.com/2014/07/21/my-own-404-not-found-making-amends-using-github/) for more details.

## Results

The output of the pipeline is a CSV file in the directory *data/est_v_nt.csv*. The file contains the top BLAST hit for human EST queries versus the nt nucleotide database, where the EST was identified using BLAT as *potentially derived* from Archaea. The last 2 columns of the file contain species and kingdom of the hit sequence. So to view only those results where the top hit was an archaeal sequence you could use, for example:

    grep Archaea data/est_v_nt.csv

## Running the pipeline

It is not straightforward to set up, so you should probably not and just trust the results :)

However, if you are feeling brave, you can try this. You'll need a Linux machine and a Ruby installation as an absolute minimum.

1. Clone this Github repository
1. Replace the 3 symbolic links in *db/* with real directories *i.e.*
<pre>
    rm db/archaea db/est db/nt
    mkdir db/archaea db/est db/nt
</pre>
Make sure there's enough room on disk for them to hold the required databases (about 50 GB as of 2014-07-22 and rising).
1. Change to the *code/ruby* directory. The *Gemfile* specifies required Ruby gems, *bio* and *nokogiri*. You can install them using Bundler:
<pre>
    cd archaeaEST/code/ruby
    bundle install
</pre>
Then run:
<pre>
    rake check
</pre>
It will tell you what tools you need to install. You'll need *seq, GNU parallel, wget, fastasplitn, blat, blastn, blastdbcmd* executables in your PATH.
1. If everything looks good, run the rake tasks in order. Remember that the BLAST databases are large and may take some time to download.
<pre>
    rake db:archaea:fetch    # Fetch archaea genome ffn files
    rake db:archaea:concat   # Uncompress archaea *.scaffold.ffn.tgz files; combine with *.ffn files
    rake db:est:fetch        # Fetch and uncompress human EST BLAST db
    rake db:est:split        # Dump human EST BLAST db to fasta, splitting for BLAT
    rake db:nt:fetch         # Fetch and uncompress nt BLAST db
    rake search:blat:run     # Run BLAT of human EST fasta files (query) vs archaea.ffn (subject)
    rake search:blat:parse   # Parse PSL files, extract list of unique human EST GI, write to file
    rake db:est:hitdump      # Dump GIs in data/est_hits_gi.txt to fasta
    rake search:blast:run    # BLAST search human EST BLAT hits to archaea vs nt database
    rake search:blast:parse  # Parse EST v nt BLAST output; use taxid to retrieve species and kingdom
</pre>

If all of that worked, you now have output in *data/est_v_nt.csv*.
