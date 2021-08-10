# neurosynth-data

This repository contains data files for use with the [Neurosynth](https://github.com/neurosynth/neurosynth) codebase.
All data are released under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/1.0/).
To a first approximation, this means you can do whatever you want with these data (including sharing, using, adapting, and modifying the data)
as long as you attribute any public use, share any derivative database under the same license, and keep any derivative data open.
See the license file for further details.

## Repository structure

All versions of the Neurosynth database (except for the 0.2 version from May 2013) are stored in this repository.

Files are named according to a number of factors, including the "vocabulary", the database version, and the type of data stored in the file.
Here is a list of relevant fields:

- Files ending in `coordinates.tsv.gz` contain the coordinates for different versions of the Neurosynth database.
  They contain one row for each coordinate.
  These files are tab-delimited and compressed, and they can be loaded with `pandas.read_table()`.
- Files ending in `metadata.tsv.gz` contain the metadata for different versions of the Neurosynth database.
  Each study (ID) is stored on its own line in the file.
  These IDs are in the same order as the `id` column of the associated `coordinates.tsv.gz` file,
  but the rows will differ because the coordinates file will contain multiple rows per study.
  They are also in the same order as the rows in any `features.npz` files for the same version.
  These files are tab-delimited and compressed, and they can be loaded with `pandas.read_table()`.
- Files ending in `features.npz` contain feature values for different types of "vocabularies".
  These files are stored as compressed, sparse matrices in order to reduce file size.
  Each file, once loaded and reconstructed into a dense matrix, contains one row per study and one column per label.
  Associated labels are stored in the files ending in `vocabulary.txt` and
  study IDs can be extracted from the associated `metadata.tsv.gz` file.
- Files ending in `vocabulary.txt` contain a list of the labels associated with the accompanying `features.npz` file.
  Each label is stored on its own line in the file.
  These files match the columns of the associated `features.npz` file.
- Files ending in `metadata.json` contain additional information about the file with the same name (except for the suffix and extension).
  For example, the metadata files for the LDA vocabularies contain descriptions about the topic modeling procedure used to generate the associated `features.npz` and `vocabulary.txt` files.
- Files ending in `keys.tsv` contain the top 100 words for each topic from the associated topic model.
  These top words can be useful when trying to summarize the topics.

## Current data

The current dataset is version 0.7, released July, 2018. Files from this version have `version-7` in the filenames.

## Vocabularies

The raw text used to annotate the Neurosynth database is the article abstract,
which we cannot share directly but which can be downloaded easily from PubMed with a tool like
[`nimare.extract.download_abstracts`](https://nimare.readthedocs.io/en/latest/generated/nimare.extract.download_abstracts.html#nimare.extract.download_abstracts).

However, the actual annotations are available in this repository.
Neurosynth currently includes two annotation approaches, resulting in 1-5 vocabularies, depending on the database version.

- `vocab-terms`: This vocabulary refers to terms extracted from abstracts using a vectorizer.
  Prior to version 0.5, these terms were simply individual words.
  However, from version 0.5 on, these terms have included both individual words (unigrams) and two-word terms (bigrams).
  Terms are extracted on a largely unsupervised basis; namely, some words are *not* extracted, based on a "stop list",
  while others may appear too frequently (e.g., common words like "the") or too infrequently to be meaningful.
  The current version's terms vocabulary is what is accessed on the Neurosynth website.
  Additionally, the values for the `features.npz` for this vocabulary are [tf-idf](https://en.wikipedia.org/wiki/Tf–idf) values, starting at version 0.4.
  In versions prior to 0.4, these values were proportions.
- `vocab-LDA[50|100|200|400]`: These vocabularies are derived from
  [latent Dirichlet allocation (LDA)](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)
  topic models fitted to the abstracts from articles in the Neurosynth database.
  LDA topic models describe texts in terms of probability distributions across "topics", which in turn are probability distributions across words.
  For more information on LDA in fMRI research, see [Poldrack et al. (2012)](https://doi.org/10.1371/journal.pcbi.1002707).
  The four vocabularies refer to different topic models with 50, 100, 200, and 400 topics, respectively.
  These vocabularies are only available for versions 0.6 and later.

## Reconstructing feature data

If you want to reconstruct the feature data into a spreadsheet-like format, you will need to combine the `features.npz`, `metadata.tsv.gz`, and `vocabulary.txt` files.

Here is some Python code that can do this, using the version 0.7's term-based features as an example:

```python
import numpy as np
import pandas as pd
from scipy import sparse

feature_data_sparse = sparse.load_npz("data-neurosynth_version-7_vocab-terms_source-abstract_type-tfidf_features.npz")
feature_data = feature_data_sparse.todense()
metadata_df = pd.read_table("data-neurosynth_version-7_metadata.tsv.gz")
ids = metadata_df["id"].tolist()
feature_names = np.genfromtxt("data-neurosynth_version-7_vocab-terms_vocabulary.txt", dtype=str, delimiter="\t").tolist()

feature_df = pd.DataFrame(index=ids, columns=feature_names, data=feature_data)
```
