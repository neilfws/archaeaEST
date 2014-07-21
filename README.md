# README

## Introduction

This repository contains code and data for discovering transcripts in the human subset of the [EST database](http://www.ncbi.nlm.nih.gov/nucest) that may originate from human-associated Archaea.

It is my attempt to make amends for the publication [An online database for the detection of novel archaeal sequences in human ESTs](http://bioinformatics.oxfordjournals.org/content/20/15/2361.abstract), which became a [404 not found](http://bioinformatics.oxfordjournals.org/content/24/11/1381.full) long ago.

## Results

The output of the pipeline is a CSV file in the directory *data/est_v_nt.csv*. The file contains the top BLAST hit for human EST queries versus the nt nucleotide database, where the EST was identified using BLAT as *potentially derived* from Archaea. The last 2 columns of the file contain species and kingdom of the query sequence. So to view only those results where the top hit was an archaeal sequence you could use, for example:

    grep Archaea data/est_v_nt.csv

## Running the pipeline

It is not straightforward to set up, so you should probably not and just trust the results :)

However, if you are feeling brave - TODO.