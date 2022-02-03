# Harmomization SOP

The aim of this document is to document a set of guidelines for harmonizing ASCT+B tables with Uberon and CL using the reports generated at https://github.com/hubmapconsortium/ccf-validation-tools/


## Table triage

How to document the state of individual tables to prioritise which work on

Example: https://github.com/obophenotype/asct-b_validation/issues/31

## Per table Issue triage:

How to structure a ticket using reports to document and prioritise different individual issues identified by the reports on a single table

Example: https://github.com/obophenotype/asct-b_validation/issues/26

To start a new issue triage comment:

1. Copy and paste the table from logs/class\_{organ}\_log.tsv into a ticket comment.  Each line is an invalid relationship (see [reference doc](#class\_{organ}\_log.tsv) for details). This should automatically format, but some editing might be needed to align columns.  Add an additional column 'n' to number issues for identification in subsequent ticket test.
2. Write notes roughly categorizing issues (identified by number from table), linking to new CL/Uberon tickets where appropriate.

### Types of errors:

 1. Simple table error - e.g.
 2. Uberon/CL missing edge -> Create CL/Uberon ticket & link to traige ticket to track e.g. 
 3. General class in specific slot in ASCT+B table e.g.
 4. Modelling disagreement -> needs to be fed back to the authors- but also create a CL or Uberon ticket as place to collect discussion e.g. https://github.com/obophenotype/uberon/issues/2126 
 5. Vasculature has special issues: TBA
 6. Resident immune cells.


### Missing Cell types

- report: 

- Minimal info table: 

- how to balance minimal info table approach with templated term addition

### Missing anatomical structures
 
- 


## Versioning and the sprint cycle


## Sprint board SOP

https://github.com/orgs/obophenotype/projects/6


# Reference docs

## Logs

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

graph
