 ## Biological Metadata Session (EMBL-EBI, 12/06/2018)
This session took place during the kickoff meeting for the ELIXIR Implementation Study entitled: “Crowd-sourcing the annotation of public proteomics datasets to improve data reusability”.

*Attending: Lydie Lane, Veit Schwämmle, Juan Antonio Vizcaíno, Fredrik Levander, Julianus Pfeuffer, Mathias Walzer*

### Overall objectives

The overall objective of this component of the project is to enable annotation a posteriori of datasets in PRIDE at the level of the experimental design (including the biological samples, replicates, etc). In principle, this will be applicable to public datasets, but we could see whether it would be feasible to apply this to private datasets as well. This infrastructure is aimed at anyone in the community, including its use internally by the PRIDE team. In parallel, the other component of the project aims to extract technical information from the submitted files (e.g. raw files, mzIdentML files, etc).

The overall main objective of the project is to improve re-usability of public proteomics datasets by third parties.

It should be noted that it is planned that the new PRIDE database system will support natively such level of biological annotation. The work included in this implementation study will help guiding those medium-term developments.

### Minutes

During the afternoon session the group split in two sub-groups in order to discuss: (i) technical aspects of automatic extraction of metadata from PRIDE submissions and, minuted in the following, (ii) facilitating a-posteriori annotation with biological metadata and combination with other metadata and submission data.

Metadata annotation needs to happen at the level of the raw files corresponding to MS runs. For these purposes, there are two types of MS runs that need to be annotated:
1. “Mono-channel” MS runs (no multiplexing) -> They contain only one sample. In this case it is easy to tag files (it is equivalent to tag samples). This case happens more often that case 2 below.
2. “Multi-channel” MS runs (multiplexing) -> They contain more than one sample in a single run (e.g. TMT, iTRAQ, etc). In an experiment of this kind, samples can get assigned to single or multiple files.

We also determined that it was important to separate two use cases: manual annotators and “data exchange” annotations (e.g. using an API). For the former, it makes intuitive sense to first group files by samples rather than annotating individual files. Doing it his way, multiple duplicate annotations for different raw files (grouped for the same sample) can be saved into one single process, saving time.

However, in a “data exchange” scenario (API), machine read and write is given, so automatic duplication of annotations where necessary is possible, independently from the file format used for performing the annotation.

For a straight-forward mapping between these two scenarios, the precise assignment of files to samples is mandatory and should be prioritized. The compared costs of designing a more parsimonious format to reflect the annotation process plus increased intricacy of implementing API and data serialisation heavily outweigh the benefits of saving marginal amounts of data compared with the proper core of submissions, yet obstruct adoption simplicity and development time.

In order to evaluate the use of different ontologies/CVs, we then discussed the use of some of them for the annotation process. Next, the discussed ontologies/CVs for proteomics experiments are listed:

| Topic | Adoption | Source | Notes |
| ----- | -------- | ------ | ----- |
| Organism | Covered in other *omics already, see ISAtab | EFO (Experimental Factor Ontology), PATO | EFO is used in other EBI resources (e.g. ArrayExpress, Expression Atlas) |
| Tissue | &uarr; | &uarr; |  |
| Disease | &uarr; | &uarr; |  |
| Sample treatment | Needs review (OLS) to add the necessary proteomics terms | EFO, sepCV | Can these be timepoints in a series? |
| Sample preparation | &uarr; | &uarr; |  |
| Instrument settings | Proteomics specific | PSI-MS |  |
| Software settings | &uarr; | &uarr; | Info about db construction or reference for reanalysis purposes |
| Software | Proteomics specific | bio.tools | Which database search engine, deconvolution, ... |
| Potentially: Optimal/used data analysis workflow | Proteomics specific | EDAM | What operations need to be used or have been used to get e.g. differentially regulated proteins |
| Named labels | Free Text Precedence | ? | E.g. Group names; free text supposed to be a name label, unique to a sample or file group, as it might have been used in a publication |


We identified “Multiplexing” as the common source of issues with connecting samples and files, as mentioned above. In this context, we evaluated the patterns prepared by ISAtab community as a possible viable solution. If ISA creation tools are not applicable without too much investment, at least the created annotation files should be convertible into (simple) ISAtab format with the lowest amount of effort imaginable.

In further discussion it became apparent that, in order to keep it simple, increase chances of adoption, and avoid (re-)introduction of MIAPE-like extensive rules, we should be as granular as possible when using CV terms that might have leaf terms, which would provide more specificity (i.e. identify sub-hierarchies in our targeted set of ontologies, which may become useful in the actual annotation process).

It was also discussed that, generally, the use of aiding procedures to select the correct CV terms is a challenging task. We will need support software that can be used to create annotations. We discussed different ways and available tools (e.g. OLS dialog [ELIXIR Norway and EBI], and ETA [ELIXIR Denmark]).

----

In the later plenary discussion (with both groups involved in the ELIXIR implementation study together) we discussed the list of CV term families that we compiled before. It was made clear that we do not intend to formulate MIAPE guidelines but rather to review the availability and usability for essential types of information in proteomics for annotation purposes.

We also briefly discussed the integration of technical metadata extracted from submission files. With respect to the needed CVs/ontologies (see table above), the PSI-MS CV should cover most cases (for instrument and software settings annotations).


### Suggested tasks:

Define ISAtab template that includes all above-mentioned topics and evaluate feasibility

Revise how available annotation tools can be adopted to this format and underlying ontologies

Look into extracting biol. Metadata from existing files (e.g. MaxQuant project files, mzid, ...)
Integration with the technical metadata groups results

Select data sets for manual annotation and testing of the to-be-developed tools

Annotate small selection of data sets and identify bottlenecks and difficulties

Write guidelines for the future annotator
