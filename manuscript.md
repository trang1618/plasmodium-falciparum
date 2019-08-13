---
author-meta:
- Alena Orlenko
- Trang T. Le
- Weixuan Fu
- Jason H. Moore
date-meta: '2019-08-13'
keywords:
- markdown
- publishing
- manubot
lang: en-US
title: 'The Malaria DREAM challenge: team TPOT''s sub-challenge 2 write-up'
...






<small><em>
This manuscript
([permalink](https://trang1618.github.io/plasmodium-falciparum/v/1fbf14f8066c176c9347a1a1b7c8460617447aff/))
was automatically generated
from [trang1618/plasmodium-falciparum@1fbf14f](https://github.com/trang1618/plasmodium-falciparum/tree/1fbf14f8066c176c9347a1a1b7c8460617447aff)
on August 13, 2019.
</em></small>

## Authors



+ **Alena Orlenko**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [0000-0003-1757-293X](https://orcid.org/0000-0003-1757-293X)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [desmidium](https://github.com/desmidium)
    · ![Twitter icon](images/twitter.svg){.inline_icon}
    [desmidium](https://twitter.com/desmidium)<br>
  <small>
     Department of Biostatistics, Epidemiology and Informatics, Institute for Biomedical Informatics, University of Pennsylvania, Philadelphia, PA 19104
  </small>

+ **Trang T. Le**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [0000-0003-3737-6565](https://orcid.org/0000-0003-3737-6565)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [trang1618](https://github.com/trang1618)
    · ![Twitter icon](images/twitter.svg){.inline_icon}
    [trang1618](https://twitter.com/trang1618)<br>
  <small>
     Department of Biostatistics, Epidemiology and Informatics, Institute for Biomedical Informatics, University of Pennsylvania, Philadelphia, PA 19104
  </small>

+ **Weixuan Fu**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [0000-0002-6434-5468](https://orcid.org/0000-0002-6434-5468)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [weixuanfu](https://github.com/weixuanfu)
    · ![Twitter icon](images/twitter.svg){.inline_icon}
    [weixuanfu](https://twitter.com/weixuanfu)<br>
  <small>
     Department of Biostatistics, Epidemiology and Informatics, Institute for Biomedical Informatics, University of Pennsylvania, Philadelphia, PA 19104
  </small>

+ **Jason H. Moore**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [0000-0002-5015-1099](https://orcid.org/0000-0002-5015-1099)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [EpistasisLab](https://github.com/EpistasisLab)
    · ![Twitter icon](images/twitter.svg){.inline_icon}
    [moorejh](https://twitter.com/moorejh)<br>
  <small>
     Department of Biostatistics, Epidemiology and Informatics, Institute for Biomedical Informatics, University of Pennsylvania, Philadelphia, PA 19104
  </small>



## Abstract {.page_break_before}

Challenge link: [https://www.synapse.org/#!Synapse:syn16924919/wiki/583955](https://www.synapse.org/#!Synapse:syn16924919/wiki/583955)

Objective: Predict the parasite clearance rate of malaria parasite isolates based on in vitro transcriptional profiles.

Data source: [@6Qb18jcS]


## Methods

### Data harmonization
For this subchallenge, the training dataset's characteristics were very different compared to those of the test set.
Importantly, training set contains *in vivo* transcription as described in [@6Qb18jcS] while test set contains *in vitro* transcription.
Other differences in data collection, synchronization, microarray platforms, preprocessing steps, etc. have been detailed on the [challenge website](https://www.synapse.org/#!Synapse:syn16924919/wiki/590948).
We first impute the missing data points using the KNN imputation method  with k = 10 via the [fancyimpute package](https://pypi.org/project/fancyimpute/).
Next, to adjust for batch effects between the two datasets, we apply the *ComBat* algorithm [@1HahRBkyb] from the *sva* R package [@TCHvrg6R] on the transcription data.
We assess the effect of the adjustment by examining the principal component analysis (PCA) plots on the raw and processed data. 

### Gene feature selection
Using the STatistical Inference Relief (STIR) algorithm [@rO22KppO], we selected the genes with adjusted STIR P values < 0.05.
Specifically, with the MultiSURF neighborhood, STIR nearest-neighbors to select features whose association with an outcome may be due to epistasis 

### Automated machine learning for model training
[TPOT details here...]
We used balanced accuracy as our scoring function.

### Test sample selection
While there is one single sample per isolate in the training set, there are eight samples for each of the 32 isolates in the test set (most have 2 biological replicates, 2 time points: 6 hours and 24 hours post invasion, perturbed with 5nM DHA (DHA) or perturbed with DMSO (UT)).
Therefore, to obtain one prediction per isolate, we need to take caution in selecting which test samples to predict on.
First, because the training samples are unperturbed, we discard the perturbed samples in the testing set, keeping only the controls (UT).
Second, we analyze the developmental stage of each biological replicate at seperate timepoints.
[...]
Transcriptional profiles in the test set were compared against the 3D7 sample from [@1Ca7UdCRk].

### Non-genetic features consideration
Because all samples from the test data were collected from the Thailand-Myanmar border, for now, we ignore the `Country` feature.
In the future, however, perhaps we can place more weights on the training samples that are geographically closer to this region.
We also ignore the `Kmeans.Grp` feature that is cluster groups corresponding to three types of transcription profile based on parasite developmental stage.

### Availability
Detailed preprocessing, modeling and analysis code for this study is available at [https://github.com/EpistasisLab/malaria-challenge](https://github.com/EpistasisLab/malaria-challenge).



## Results
### Batch effect adjustment
Before batch adjustment, the two datasets are clearly separated in the first two principal component dimensions (Fig. {@fig:PACbatch}A).
After being adjusted for batch effect, this dataset-specific effect is less evident (Fig. {@fig:PACbatch}B).
The amount of variance explained in each component also seems to be more balanced.
![Principal component analysis plots before (A) and after (B) adjusting for batch effects](images/PCA_for_batch_effect.png){#fig:PACbatch width="100%"}

The training dataset consists of 1043 in vivo parasite isolates, each with 4952 transcriptomic features, out of which STIR selects 1068.

TPOT cross-validated balanced accuracy...

### Test sample selection
In general, the parasite developmental stages in the training set are smaller than those in the test set (Fig. {@fig:stages}).
![Developmental stages in training set and testing set](images/dev-stage.png){#fig:stages width="100%"}






## Conclusion

### Take home
Even with automated machine learning, at the moment, we still need to be clever at preprocessing and integrating the data.


## References {.page_break_before}

<!-- Explicitly insert bibliography here -->
<div id="refs"></div>
