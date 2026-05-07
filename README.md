# igv_variant_inspection
This project provides a directly applicable protocol for inspecting short-read alignments in IGV (Integrative Genomics Viewer), covering three core tasks: visual verification of called variants, coverage analysis, and identification of mismatches. The workflow was developed using Illumina reads from a green macroalga aligned against its reference genome. Each step is documented to be reproducible on any BAM/reference pair, regardless of organism. The result is a concise, hands-on reference for quality-controlling variant calls before downstream analysis. It is aimed at users who need to move beyond automated caller output and confirm what the reads actually support.

The main steps covered:

1. Starting with raw data & Quality control 
2. generate a GC track
3. Generate coverage information
4. Visualize data in IGV

Go to docs and start with 01_qualitycontrol.md

```
├── README.md
├── docs/
│   ├── 01_qualitycontrol.md 
│   ├── 02_GCtrack.md 
│   ├── 03_coverage.md
│   └── 04_visualization_in_igv.md
└── images/
```
