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
The latest data file is always stored in current_data.tar.gz in the root folder (and mirrored in the archive/ folder).

The current dataset is version 0.4, released on September 22, 2014. The archive contains two files: database.txt and features.txt. The database.txt file contains activation data for over 9,700 studies. The features.txt file contains feature information for nearly 3,000 term-based features. Note that unlike previous feature data releases (prior to v0.3), the current release contains term-based features derived only from the abstracts of articles in the Neurosynth database, and not from the full text. Thus, a value of, say, 0.03 for a feature like "emotion" would indicate that the term 'emotion' constitutes 3% of all word tokens in the abstract of a given study.

Additionally, unlike previous releases, the present feature set includes not only single word features, but also N-gram features extracted from the Cognitive Atlas (e.g., "working memory", "emotion regulation", etc.).

**IMPORTANT NOTE**: Beginning with version 0.3, studies in both the features and database files are identified by PubMed ID (rather than the doi used previously). This means that if you've previously (i.e., pre-April 2014) generated a Dataset instance using an older database.txt file, you'll need to re-generate the Dataset before loading new features.

Archive
-------
The archive folder contains all releases to date. Each tarred and gzipped archive contains a pair of database.txt and features.txt files. Currently archived data include:

* archive/data_0.2.May_2013.tar.gz: These data files were in use from May 2013 to April 2014. The database.txt file contains activation data for approximately 5,300 studies. The features.txt files containsfeature data for 525 manually-selected terms. These weights were derived from the full-text of each article. Thus, a value of, say, 0.003 for a feature like "emotion" would indicate that the term 'emotion' constitutes 0.3% of all word tokens in the full text of an article in the database. The first (identifying) column is the doi of each article.

* archive/data_0.3.April_2014.tar.gz: These data files were in use from April 2014 to September 2014. The database.txt file contains activation data for approximately 8,000 studies. The features.txt files containsfeature data for nearly 3,000 automatically extracted terms. These weights were derived from only the abstract of article. Thus, a value of, say, 0.003 for a feature like "emotion" would indicate that the term 'emotion' constitutes 0.3% of all word tokens in the article abstract. The first (identifying) column is the PubMed ID (PMID) of each article.

* archive/data_0.4.September_2014.tar.gz: These data files are currently in use (as of September 2014). The database.txt file contains activation data for over 9,700 studies. The features.txt files contains feature data for nearly 3,000 automatically extracted terms, including both single words and N-grams extracted from the Cognitive Atlas vocabulary. These weights were derived from only the abstract of article. In contrast to previous datasets, the feature weights are now normalized, and reflect [tf-idf](http://en.wikipedia.org/wiki/Tf%E2%80%93idf) values rather than proportions. The first (identifying) column is the PubMed ID (PMID) of each article.
