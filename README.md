neurosynth-data
===============
This repository contains data files for use with the [Neurosynth](https://github.com/neurosynth/neurosynth) codebase.

Repository structure
--------------------
The most recent version of the Neurosynth datafiles are always contained in the root folder. Key files include:

* database.txt: a plaintext file containing activation data and key metadata for all studies in the Neurosynth database. Each row represents a single activation from a single study. Mandatory columns include PMID, x/y/z coordinates, and stereotactic space. The remaining columns contain useful but non-essential metadata (e.g., author names, journal title, etc.). These latter columns can be deleted if desired.
* features.txt: a plaintext file containing feature information for studies in database.txt. Each row represents a single study. The first column contains the PMID of the study and is used to map data in features.txt to the data in database.txt. Each column contains the weights for a different feature. Titles for all features are provided in the first row.

Current data
------------
The current dataset (version 0.3) was released on April 4, 2014. The database.txt file contains activation data for over 8,000 studies. The features.txt file contains feature information for nearly 3,000 term-based features. Note that unlike previous feature data releases, the current release contains term-based features derived only from the abstracts of articles in the Neurosynth database, and not from the full text. Thus, a value of, say, 0.03 for a feature like "emotion" would indicate that the term 'emotion' constitutes 3% of all word tokens in the abstract of a given study.

Archive
-------
The archive folder contains past data releases. Each tarred and gzipped archive contains a pair of database.txt and features.txt files. Currently archived data include:

* data_0.1_May_2013.tar.gz archive/data_0.2.May_2013.tar.gz: These data files were in use from May 2013 to April 2014. The database.txt file contains activation data for approximately 5,300 studies. The features.txt files containsfeature data for 525 manually-selected terms. These weights were derived from the full-text of each article. Thus, a value of, say, 0.003 for a feature like "emotion" would indicate that the term 'emotion' constitutes 0.3% of all word tokens in the full text of an article in the database.