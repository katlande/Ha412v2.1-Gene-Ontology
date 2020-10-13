# Ha412v2.1-Gene-Ontology
Functions and Libraries for running GO functional analysis in the Ha412v2.1 reference genome


# This repository contains the script and all necessary support files for running GO Functional Analysis on genes from the Ha412v2.1 *Helianthus* reference genome.


**Dependencies:** 

1. The script requires the packages "stats" and "GO.db"
2. Script must be run from the "GO_Libraries" folder


**Gene Ontology is conducted by simply running the run_helianthus_GO() function, which has the following parameters:**

1. query_vector: A character vector of genes in which to search for GO enrichment
2. library: the genes the query_vector will be search against. Default = "Genome". Options = ("Genome", "Subset"). "Subset" used if only a selection of genes are to be used as the library. Use of "Subset" requires a vector of gene names to be input in the "subset_library" option. 
3. ontology: Options = "BP" (Biological Process), "MF" (Molecular Function), "CC" (Cellular Compartment). Default = "BP"
4. include.gene.names: Boolean. Default = T. Whether or not the names of genes with each GO term should be included in the output file.
5. p.adj: Options: see p.adjust.methods. Default = "fdr"
6. Source: Source of GO terms. Options = "XRQ" (GO terms from XRQ homolog blast2GO only), "TAIR" (GO terms from TAIR homologs), or "Merged" (GO terms from TAIR homologs, XRQ homologs, and InterPro terms). Default = "Merged". The "XRQ" library features GO with a maximum of 2 parent node steps away from directly annotated GO terms. The "TAIR" library contains all GO terms directly annotated to Tair homologs of Ha412 genes, without ancestral nodes added. Due to the variation in annotation methods of Tair contributers, annotations may follow varying paradigms and thus may not be entirely representitive. In the "Merged" annotations, ALL ancestral terms are used as the mixed annotation paradigms for TAIR, XRQ, and InterPro cause a lot of variability and including all terms is the best way to minimize resultant noise. The "Merged" GO library thus contains far more GO terms than the others, and will be more likely to give false positives. When using this library, use of a more stringent filtering threshold, statistical test, and p.adjustment method would be wise. 
7. subset_library: If entire genome is not being used as the background library, input a character vector of gene names to be used as the background library. Subset library must contain ALL genes in the query, otherwise results may be problematic.
8. filter: p-adjust threshold above which GO terms should be discarded from the output file. Default = 0.05.
9. statistic: Which test to use to generate a p-value. Options = "Fisher" (Fisher's Exact Test), "Chi" (Chi-squared). Default = "Fisher", Fisher's test will give more conservative p-values than Chi-squared. 
10. specific_terms_only: Boolean. Whether or not to remove non-specific GO terms from the analysis. Requires and input for term_removal_distance. Deafult = FALSE.
11. term_removal_distance: numeric, one of (1,2,3). When using the specific_terms_only = TRUE option, term_removal_distance allows the user to determine how specific output terms will be. 1, 2, and 3 refer to term distances from the overall ontology annotation. For example, term_removal_distance=1 in a "BP" ontology would remove all GO terms that are direct children of the "biological_process" annotation. term_removal_distance=2 removes direct children of the "biological_process" annotation, and the children of direct children of of the "biological_process" annotation. 1 removes the least specific terms, while 3 removes more specific terms. Specifically useful when using the "merged" library which is prone to false positives. Default = NULL.


### Examples of how to run the function:

**query some random genes against the genome for biological process:**
`query <- dplyr::sample_n(all_genes, 1000)
query <- query$Gene
GO_output <- run_helianthus_GO(query_vector=query, include.gene.names=F)`

**query some random genes against a subset of the genome:**
`lib <- dplyr::sample_n(all_genes, 10000)
lib <- lib$Gene
lib <- unique(c(query, lib))
GO_output <- run_helianthus_GO(query_vector=query, library="Subset", subset_library=lib, include.gene.names=F)`

**include gene names in output file:**
`GO_output <- run_helianthus_GO(query_vector=query)`

**Chi-squared test:**
`GO_output <- run_helianthus_GO(query_vector=query, include.gene.names=F, statistic = "Chi")`

**p-value cut-off of 0.001:**
`GO_output <- run_helianthus_GO(query_vector=query, filter = 0.001, include.gene.names=F)`

**Molecular function ontology:**
`GO_output <- run_helianthus_GO(query_vector=query, ontology = "MF", include.gene.names=F)`

**only look at more specific GO terms:**
`GO_output <- run_helianthus_GO(query_vector=query, include.gene.names=F, specific_terms_only=T, term_removal_distance = 3)`

**XRQ library:**
`GO_output <- run_helianthus_GO(query_vector=query, Source = "XRQ", include.gene.names=F)`



Last update: July 17th, 2020
Contact: Kathryn.Lande@mail.mcgill.ca
