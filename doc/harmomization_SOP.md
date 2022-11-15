# Harmomization SOP

The aim of this document is to document a set of guidelines for harmonizing anatomical structures, cell types plus biomarkers (ASCT+B) tables described in [(Börner et al. 2021)](https://doi.org/10.1038/s41556-021-00788-6) with Uberon and CL using the reports generated at https://github.com/hubmapconsortium/ccf-validation-tools/ The validation pipeline is documented [here](https://github.com/hubmapconsortium/ccf-validation-tools/blob/master/docs/Pipeline_functional_spec.md).

## Types of reports generated

## Logs folder 
1. The logs folder contains files with this naming schema: **class\_{organ}\_log.tsv**

These files contain a table of nonvalidating* relationships: s = subject_id; slabel = rdfs:label in ontology; user_slabel= author preferred label of subject in ontology.; o = object etc. The user_slabel sometimes differs from that found in the ontology. These appear as red edges in the graphs.

2. The logs folder contains files with this naming schema: **new_cl_terms_{organ}.tsv**

This file Terminal provides of table of the terminal anatomical structure listed in the ASCT+B tabble, and cell types not currently found in Cell Ontology (CL). 

AS/ID	Terminal AS/label	Terminal AS/user_label	CL Name	References/ID	References/DOI

<img width="598" alt="Screen Shot 2022-02-07 at 6 32 38 PM" src="https://user-images.githubusercontent.com/47257000/152889950-e28536c2-2535-4fcd-9eeb-320ffe7ef434.png">


3. The logs folder contains files with this naming schema: **error_class_{organ}.log**

This file provides general quality check of data table. Blank spreadsheet cells often mean no ontology mapping found by author. Foundational model of anatomy ontology IDs are provided when an adequate term is not found in Uberon. Typos, font case (upper case), punctuation mistakes in IDs: two colons, spaces, underscore instead of colon are all found in this error log.

WARNING - No valid ID provided for '',

WARNING - Different labels found for CL:XXXXXXX

WARNING - Different labels found for UBERON:XXXXXXX

WARNING - Unrecognised UBERON/CL entity 'CL:XXXXXXX'

WARNING - Unrecognised cell content 'FMA_7206'


4. The logs folder contains files with this naming schema: **{organ}_AS_CT_strict_log.tsv**

5. The logs folder contains files with this naming schema: **{organ}_AS_has_part_CT_log.tsv**

### class\_{organ}\_log.tsv

A table of nonvalidating* relationships: s = subject_id; slabel = label in ontology; user_slabel= label of subject in ontology.; o = object etc.
These appear as red edges in the graphs.

 s | slabel | user_slabel | o | olabel | user_olabel
-- | -- | -- | -- | -- | --
   CL:0011005 | GABAergic interneuron | GABAergic interneuron | CL:0000561 | Amacrine cell | Amacrine Cell
   CL:0011005 | GABAergic interneuron | GABAergic interneuron | UBERON:0001791 | inner nuclear layer of retina | inner nuclear layer of retina

\* nonvalidating means does not validate against the following relationships *post reasoning*.  i.e. the relationship validated may not be directly stated in the source ontology, but is safely inferred.
AS:AS  - subClassOf; part_of; connected_to; overlaps (only checked if part_of is not true).
AS:CT  - part_of - we also test for has_part (reversing subject and object); perhaps controversially, we validate has_part if some cell that if some cell for which part_of is true is a subclass of the type in the table.
CT:CT  - subClassOf; develops_from. # Should we even be doing this??!!  

## Graphs

The graphs folder contains files with the naming schema: ccf_{organ}_graph.png

Red edges in the graphs indicate relationships not currently found in the ontology (nonvalidating). ccf_located_in (for cell types) or ccf_part_of (for anatomical structures) are relationships designed to capture relationships ASCT+B authors provided for their organ of interest.

Blue edges are part_of relationships. 

Black edges are rdfs:subClassOf

Example of the eye graph:

<img width="602" alt="Screen Shot 2022-02-07 at 5 26 25 PM" src="https://user-images.githubusercontent.com/47257000/152883080-ea2ba917-908f-4d04-a486-2b93874f5869.png">

## Owl


## Reports
https://github.com/hubmapconsortium/ccf-validation-tools/tree/master/reports

A folder containing weekly status report summary table for all ASCT+B tables. The table reports: table (name of ASCT+B table), number_of_AS-AS_relationships, percent_invalid_AS-AS_relationship, percent_indirect_AS-AS_relationship, number_of_CT-CT_relationships, percent_invalid_CT-CT_relationship,	percent_indirect_CT-CT_relationship,	number_of_CT-AS_relationships,	percent_invalid_CT-AS_relationship

file naming schema: report_relationship_{date (YYYYMMDD}.tsv

<img width="986" alt="Screen Shot 2022-02-07 at 6 01 12 PM" src="https://user-images.githubusercontent.com/47257000/152886792-5725a353-14f1-4842-a8b1-39283d7b3a48.png">



## Table triage
How to document the state of individual tables to prioritise which to work on

A summary of ASCT+B table reports, graph, unvalidated edges, new cell types, brief notes and prioritization based on types of issues is represented here: 
Example: https://github.com/obophenotype/asct-b_validation/issues/31

## Per table Issue triage:

How to structure a ticket using reports to document and prioritise different individual issues identified by the reports on a single table

Example: https://github.com/obophenotype/asct-b_validation/issues/26

To start a new issue triage comment:

1. Copy and paste the table from logs/class\_{organ}\_log.tsv into a ticket comment.  Each line is an invalid relationship (see [reference doc](#class\_{organ}\_log.tsv) for details). This should automatically format, but some editing might be needed to align columns.  Add an additional column 'n' to number issues for identification in subsequent ticket test.
2. Write notes roughly categorizing issues (identified by number from table), linking to new CL/Uberon tickets where appropriate.

### Types of errors:

 1. Simple table error - e.g. upper vs lower case of label, uberon: vs uberon_, uberon::, misspellings (these can be corrected directly by MC-IU team)
 2. Uberon/CL missing edge -> Create CL/Uberon ticket & link to triage ticket to track e.g. 
 3. General class in specific slot in ASCT+B table e.g. typically broad classes like "vasculature", "lymph", "immune" etc
 4. Modelling disagreement -> needs to be fed back to the authors- but also create a CL or Uberon ticket as place to collect discussion e.g. https://github.com/obophenotype/uberon/issues/2126 
 5. Vasculature has special issues: TBA
 6. Tissue resident immune cells reported and defined as "lymphocytes that do not recirculate in the blood or lymphatic system and often adopt a unique phenotype that is distinct from those of circulating immune cells"; ASCT+B organ experts agree that these are not simply contamination from blood vessel populations. See [Takamura, 2018](https://www.frontiersin.org/article/10.3389/fimmu.2018.01214) and [Sun, 2019](https://doi.org/10.1038/s41423-018-0192-y)


### Missing Cell types

- report: 

- Minimal info table: [Author Initiated New Term Requests](https://docs.google.com/spreadsheets/d/1BihrOU9vWrsA_wYjcbdPUr42_Sb22CO3gkBDieiYQPs/edit?usp=sharing)

- how to balance minimal info table approach with templated term addition

### Missing anatomical structures
 
- 


## Versioning and the sprint cycle


## Sprint board SOP

https://github.com/orgs/obophenotype/projects/6

## ASCT+B table versions

[Landing page for all versions of ASCT+B tables](https://docs.google.com/document/d/1vW9NI17EIMPrTOW27nDHEM8ZTMd7ZqAV232taIXfH8c/edit#heading=h.9etsl5gwny23)
