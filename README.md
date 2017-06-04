neurosynth-data
===============
This repository contains data files for use with the [Neurosynth](https://github.com/neurosynth/neurosynth) codebase. All data are released under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/1.0/). To a first approximation, this means you can do whatever you want with these data (including sharing, using, adapting, and modifying the data) as long as you attribute any public use, share any derivative database under the same license, and keep any derivative data open. See the license file for further details.

Repository structure
--------------------
The most recent version of the Neurosynth datafiles are always contained in the root folder. Key files include:

* database.txt: a plaintext file containing activation data and key metadata for all studies in the Neurosynth database. Each row represents a single activation from a single study. Mandatory columns include PMID, x/y/z coordinates, and stereotactic space. The remaining columns contain useful but non-essential metadata (e.g., author names, journal title, etc.). These latter columns can be deleted if desired.
* features.txt: a plaintext file containing feature information for studies in database.txt. Each row represents a single study. The first column contains the PMID of the study and is used to map data in features.txt to the data in database.txt. Each column contains the weights for a different feature. Titles for all features are provided in the first row. Additionally, in contrast to previous datasets, the feature weights are now normalized, and reflect [tf-idf](http://en.wikipedia.org/wiki/Tf%E2%80%93idf) values rather than proportions.

Current data
------------
The latest data file is always stored in current_data.tar.gz in the root folder (and mirrored in the archive/ folder).

The current dataset is version 0.6, released July, 2015. The archive contains two files: database.txt and features.txt. The database.txt file contains activation data for 11,406 studies. The features.txt file contains feature information for over 3,300 term-based features. Note that unlike previous feature data releases (prior to v0.3), the current release contains term-based features derived only from the abstracts of articles in the Neurosynth database, and not from the full text. This change was introduced in order to improve specificity and reproducibility of results.

Additionally, unlike previous releases, the present feature set includes not only single word features, but also N-gram features (e.g., "working memory", "emotion regulation", etc.).

**IMPORTANT NOTE**: Beginning with version 0.3, studies in both the features and database files are identified by PubMed ID (rather than the doi used previously). This means that if you've previously (i.e., pre-April 2014) generated a Dataset instance using an older database.txt file, you'll need to re-generate the Dataset before loading new features.

Archive
-------
The archive folder contains all releases to date. Each tarred and gzipped archive contains a pair of database.txt and features.txt files. Currently archived data include:

* archive/data_0.2.May_2013.tar.gz: These data files were in use from May 2013 to April 2014. The database.txt file contains activation data for approximately 5,300 studies. The features.txt files containsfeature data for 525 manually-selected terms. These weights were derived from the full-text of each article. Thus, a value of, say, 0.003 for a feature like "emotion" would indicate that the term 'emotion' constitutes 0.3% of all word tokens in the full text of an article in the database. The first (identifying) column is the doi of each article.

* archive/data_0.3.April_2014.tar.gz: These data files were in use from April 2014 to September 2014. The database.txt file contains activation data for approximately 8,000 studies. The features.txt files containsfeature data for nearly 3,000 automatically extracted terms. These weights were derived from only the abstract of article. Thus, a value of, say, 0.003 for a feature like "emotion" would indicate that the term 'emotion' constitutes 0.3% of all word tokens in the article abstract. The first (identifying) column is the PubMed ID (PMID) of each article.

* archive/data_0.4.September_2014.tar.gz: The database.txt file contains activation data for over 9,700 studies. The features.txt files contains feature data for nearly 3,000 automatically extracted terms, including both single words and N-grams extracted from the Cognitive Atlas vocabulary. These weights were derived from only the abstract of article. In contrast to previous datasets, the feature weights are now normalized, and reflect [tf-idf](http://en.wikipedia.org/wiki/Tf%E2%80%93idf) values rather than proportions. The first (identifying) column is the PubMed ID (PMID) of each article.

* archive/data_0.5.February_2015.tar.gz: The format and structure of all files in this release is identical to the previous one (0.4); the only difference is the addition of new studies and features. The 0.5 release contains data for > 10,900 studies and > 3,300 features.

* archive/data_0.6.July_2015.tar.gz: The format and structure of all files in this release is identical to the previous one (0.5); the only difference is the addition of new studies and features. The 0.6 release contains data for > 11,406 studies and > 3,100 term-based features. Note that while the total number of studies in this release is only slightly larger than in the previous release, new filtering heuristics were introduced to better exclude non-fMRI studies. Thus, there are actually approximately 1,500 new studies (whereas approximately 1,000 studies found in the last release were removed).
