# European Bioconductor Developers' [Meeting 2015](https://sites.google.com/site/eurobioc2015/), Cambridge UK

## Learning from heterogeneous data sources
Lisa Breckels

Spatial proteomics
*	Spatial proteomics is the systematic large-scale analysis of a cell’s proteins and their assignment to distinct sub-cellular compartments  
*	It is vital for deciphering a proteins function and possible interaction partners
*	It is important in identifying therapeutic targets and the design of drugs as it there is a significant correlation between protein mis-localisation and disease

Localisation maps
*	There are a number of ways we can go about studying protein localisation and one approach is to use quantitative proteomics methods
*	Very generally, this involves taking cells, gently breaking them to extract their cellular content (but without disrupting the organelles inside), separating on a density gradient, taking discrete fractions across the gradient, and then using MS to identify the content. 
*	The result of these types of experiments is a matrix of quantitation values, where we have proteins along the rows and their respective relative intensity in each fraction along the columns
*	We find proteins belonging to the same organelle share similar density gradient patterns
*	To assign proteins to a sub-cellular localisation we can use supervised machine learning. This requires a set of known residents to use as training examples.

Sources of information
*	In addition to quantitative proteomics there are other sources of information that we can use to assign a protein to its sub-cellular location these include: GO terms,	Microscopy images, Amino acid sequences properties etc.
*	These sources of information as widely and freely available in the public domain

Our goal
*	Our goal is to see if we can use these auxiliary data sources to support and complement our primary task 
*	We have to be careful combining data as they are very different:
*	The experimental data is species and condition specific, may be targeted to a specific biological question e.g. resolving a specific organelle of interest
*	Auxiliary data is often more general/static, has a low signal-to-noise ratio
* Must be	careful not to dilute high quality information, simple `cbind` does not work, use of auxiliary data may be organelle specific
*	USE TL

Two classifiers
*	K-NN TL: uses optimised data source-specific weights and class-specific weights for the primary and auxiliary data,	allows us to use as little or as much of the auxiliary information to complement our experimental data on a class-by-class basis
*	SVM TL: similarly SVM TL uses data specific kernels and data specific parameters
*	The output from these classifiers is a class prediction and classifier score. Using a score threshold we can decide whether to assign a protein to its predicted compartment or leave unassigned.

Biological application
*	We tested these classifiers on 5 different datasets from 4 different species in conjunction with there different sources of information and found that combining data always did significantly better than training on experimental data alone
*	We applied these classifiers to classify novel proteins in mouse e14 pluripotent stem cells
* LHS: PCA plot primary data, we train a classifier on experimental data only and classify the unlabelled data (grey)
*	Middle: PCA plot auxiliary data, GO terms
*	Then we run a TL classifier using both P and A, and classify the unknowns
*	RHS: take a very recent dataset (2015) on the same cell line which has more proteins, better resolution, better separation, new experimental design and use it as a gold standard on which to validate our predictions on the 2011 data

Boxplot
*	We examine the correct/incorrect assignments (according to the new mouse map), made using all classifiers and plot their score distributions
*	LHS: here we have k-NN on P only with k-NN TL that uses P + A
*	RHS: here we have  SVM on P only with SVM TL that uses P + A
*	Note: the score values are irrelevant as scores are classifier specific, what is important is the distribution of scores
*	E.g. SVM, RHS, we find that with the TL method we get much better discrimination between correct and incorrect assignments. 
*	This is very important, because the output of all these classifiers is just a score for each protein, and before doing any biological interpretation we need to set a threshold on which we decide whether to assign a localisation, or leave unlabelled, which is actually quite difficult.
*	Here we see that if we just take a threshold of say 0.6 we have very low false positive rate using TL, but if we take the same threshold for the SVM we see we get higher FDR as there is a large overlap in the distribution of scores for correct and incorrect assignments 
*	Using TL we lower FDR

Infrastructure
*	All this implemented in pRoloc software, including vignette on how to apply the algorithms, all theories explained in the pre-print  

Slides under a CC-BY license.
