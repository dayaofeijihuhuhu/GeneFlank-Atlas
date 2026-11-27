# GeneFlank Atlas

**An interactive, browser-based atlas for exploring antimicrobial resistance gene (ARG) neighborhoods and their genomic contexts.**

GeneFlank Atlas visualizes the arrangement, orientation, and annotations of genes surrounding resistance loci. It supports cluster-specific exploration, sequence comparisons, and summaries of the source composition of each ARG cluster. The application runs entirely in the browser and can also be used offline.

**Web application:** https://dayaofeijihuhuhu.github.io/GeneFlank-Atlas/  
**Repository:** https://github.com/dayaofeijihuhuhu/GeneFlank-Atlas

## Features

- **ARG cluster exploration:** Load a TSV dataset and investigate one ARG cluster at a time.
- **Gene neighborhood visualization:** Display gene order, genomic coordinates, orientation, annotation database, and nearby genes. Arrows indicate gene orientation, colors indicate annotation database, and stars mark recognized resistance genes.
- **Interactive filtering:** Search genes or sequences; select sequences and resistance genes; filter by genomic location and predicted plasmid mobility; and adjust the flanking range (0–20 kb).
- **Sequence comparison:** Compare 2–4 selected sequences on a shared coordinate scale, with synchronized zooming and horizontal navigation.
- **Gene annotation details:** Click a gene arrow to inspect its annotations and coordinates.
- **Cluster summaries:** View sequence counts, resistance-gene counts, flanking-gene counts, insertion-sequence annotations, genomic-location and mobility distributions, and source composition.
- **Export:** Save the current visualization as SVG or PNG, or export filtered gene records as TSV.
- **Local processing:** TSV files are processed in the browser; no server upload is required. A background worker handles file parsing, and the selected cluster is loaded for exploration.

## Getting started

1. Open the [GeneFlank Atlas website](https://dayaofeijihuhuhu.github.io/GeneFlank-Atlas/) or download and open `index.html` in a modern browser.
2. Click **Import TSV** or drop a UTF-8, tab-separated file into the upload area.
3. Choose an ARG **Cluster** from the left panel.
4. Use the search and filter controls to select genomic contexts of interest.
5. Explore **Sequence view**, click gene arrows for details, or select **2–4 sequences** and open **Compare**.
6. Export figures as **SVG/PNG** or records as **TSV**.

The website starts with an empty workspace. **No example dataset is automatically loaded.**

## Input data format

Input must be a **UTF-8 tab-separated file (`.tsv`)** with one row per annotated gene.

### Required fields

| Column | Description |
| --- | --- |
| `gene` | Gene name or annotation. |
| `start` | Nonnegative integer start coordinate. |
| `end` | Nonnegative integer end coordinate. |
| `strand` or `orientation` | Gene orientation: `1`, `-1`, `+`, or `-`. |
| `db_name` | Annotation database, used for gene colors (e.g., `ResFinder`, `ISfinder`, `UniProtKB`). |

At least **one** sequence identifier column must also be present: `file`, `seq`, or `seq_id`. Using both `file` and `seq` is recommended to distinguish sequences across genomes. 

### Recommended additional fields

| Column | Description |
| --- | --- |
| `cluster` | ARG cluster; missing or blank values are grouped as `Unassigned`. |
| `file` | Genome or assembly identifier. |
| `seq` | Contig or sequence identifier. |
| `seq_id` | Alternative sequence identifier. |
| `gene_short` | Short gene label displayed in the plot; legacy `gene2` is also accepted. |
| `drug_class` | Antimicrobial drug class. |
| `location` | Genomic location, such as chromosome or plasmid. |
| `predicted_mobility` | Predicted mobility class. |
| `source` | Source category for cluster composition and filtering context. |
| `serovar` or `serotype` | Serovar/serotype annotation. |
| `label` | Optional label carried in the input dataset. |


### Illustrative TSV example

```tsv
gene	start	end	strand	db_name	gene_short	file	seq	cluster	location	predicted_mobility	source	serovar
blaTEM-1	1000	1860	1	ResFinder	blaTEM-1	Genome_001	Contig_01	blaTEM-1A	Plasmid	Conjugative	Human	Typhimurium
tnpA	1950	2900	-1	ISfinder	tnpA	Genome_001	Contig_01	blaTEM-1A	Plasmid	Conjugative	Human	Typhimurium
```

*These rows are illustrative only and do not represent an included dataset.*

## Visualization and interpretation

- Gene arrows reflect the supplied coordinates and orientation.
- Gene fill colors correspond to `db_name`, rather than an inferred gene function. Recognized resistance genes are highlighted with a star; recognition currently includes `ResFinder` annotations and `mcr-`-prefixed genes.
- **Flanking range** limits the displayed window around recognized resistance-gene anchors. The display cannot recover annotations absent from the input TSV.
- **Source composition** is calculated for the **entire selected cluster**, before additional sequence filters. Each unique sequence is counted once, rather than once per gene; unknown sources are included in the denominator.
- Sequence-distribution summaries for genomic location and predicted mobility reflect the currently filtered sequences.
- Exported TSV records retain the original input columns and reflect the current cluster and applicable filters.

## Running and deployment

GeneFlank Atlas is a self-contained HTML application with an embedded copy of **D3.js v7.9.0**. No installation, package manager, or backend service is needed.

To deploy using GitHub Pages, place `index.html` at the root of the repository and configure **Settings → Pages → Deploy from a branch → `main` / `(root)`**.

## Notes

GeneFlank Atlas is intended for **exploration and visualization** of input annotations. It does not itself establish horizontal gene transfer, transmission direction, or clinical resistance phenotypes. Results depend on the completeness and accuracy of the supplied annotations and metadata.
