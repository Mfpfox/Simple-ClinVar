# SCV
Shiny ClinVar web server pre-filtering stage and R source code of http://scv.broadinstitute.org/

# Prefiltering stage

Input: download variant_summary.txt from ftp://ftp.ncbi.nlm.nih.gov/pub/clinvar/tab_delimited/variant_summary.txt.gz and decompress
Run: $perl clinvar.pre-filtering.pl variant_summary.txt

The program performs the following taks:

First, we keep only entries from the human reference genome version GRCh37.p13/hg19 and referring to canonical transcripts. 

Second, Molecular consequence is inferred through the analysis of the Human Genome Variation Society (HGVS) sequence variant nomenclature field. HGVS format contains string patterns that allow molecular consequence inference (e.g. ENST00001234 c.3281 [p.Gly1046Arg]). Specifically, when the variant is reported to cause an amino acid change different to the reference it is annotated as a “missense” variant (e.g. p.Gly1046Arg). If the genetic variant leads to the same amino acid (e.g. p.Gly1046Gly) or a stop codon (e.g. p.Gly1046*) the entry is annotated as “synonymous” or “stop-gain” variant, respectively. Depending on the observed outcome, small insertions and deletions molecular consequence (collectively “indels”; e.g. p.Gly1046Glyfs.*, p.Gly1046Glyins.*, p.Gly1046del.*) are separated in to “frameshifts” and “in-frame indels”. 

Third, we reduced the complexity of the clinical significance field by regrouping and merging them in to five unique and non-redundant categories: “Pathogenic”, “Likely pathogenic”, “Risk factor and Association”, “Protective/Likely benign” and “Benign”. Conflicting interpretations of pathogenicity, variants of unknown significance (VUS) and contradictory evidence (e.g. “Likely benign” alongside “Risk” evidences) were combined together in to an “Uncertain/Conflicting” category. Similarly, variants annotated with multiple evidence categories of the same evidence direction such as “Pathogenic” alongside “Likely pathogenic” were combined and the respective lower evidence category assigned. Fourth, ClinVar entries with phenotypes annotated as “not provided” and “not specified” were combined in to one single category called “Not provided / Not specified”.

Output: clinvar.[month.of.release].pf that serves as an input for the shiny app code of Shiny ClinVar web server hosted at http://scv.broadinstitute.org/

# Shiny ClinVar Source code

User interface: ui.R
Server code: server.R

Input1: clinvar.[month.of.release].pf
Input2: genes.refSeq
Input3: gene-ccds-seq-length-uniprot.txt

Usage:

Step 1: place on the same folder Input1, Input2, Input3, ui.R and server.R

Step 2: on R studio run server.R or ui 
