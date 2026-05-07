# Step 3 — Mapping and IGV tracks

Maps the trimmed Illumina reads back against the assembled organelle genome
to generate a sorted, indexed BAM file for IGV inspection. In parallel, a
GC-content track is computed from the assembly itself, providing a reference
signal that helps interpret coverage anomalies during visual inspection.

## Tools

| Tool     | Purpose                                            |
|----------|----------------------------------------------------|
| samtools | FASTA indexing, BAM sorting and indexing           |
| bedtools | window generation and per-window GC calculation    |
| bowtie2  | short-read alignment against the assembly          |

## Setup (once)

```bash
mamba env create -f 3_mapping_and_tracks/environment.yml
mamba activate mapping
```

## Run

From the repository root:

```bash
bash 3_mapping_and_tracks/run_mapping.sh
```

Inputs:
- Assembly: `2_organelle_assembly/macroalga.getorganelle.fasta`
- Trimmed reads: `1_quality_control/trimmed/seq_{1,2}.trimmed.fastq.gz`

## Pipeline

1. **Index the assembly** (`samtools faidx`) — required for window generation
2. **Create 50 bp windows** across the assembly with `bedtools makewindows`
3. **Calculate GC content** per window with `bedtools nuc`
4. **Convert to bedGraph** and sort for IGV compatibility
5. **Build Bowtie2 index** from the assembly
6. **Align paired-end reads** against the assembly, suppress unmapped reads
7. **Filter and sort** the BAM file
8. **Index the BAM** for random access in IGV

## Outputs

| File                                                              | Use in IGV                  |
|-------------------------------------------------------------------|-----------------------------|
| `3_mapping_and_tracks/results/macroalga.50bp.gc.sorted.bedGraph`  | GC-content track            |
| `3_mapping_and_tracks/results/mapping/macroalga_sorted.bam`       | alignments, coverage        |
| `3_mapping_and_tracks/results/mapping/macroalga_sorted.bam.bai`   | BAM index (auto-loaded)     |
| `3_mapping_and_tracks/results/mapping/align_stats.txt`            | Bowtie2 alignment summary   |

## Key parameters

| Parameter            | Value | Rationale                                       |
|----------------------|-------|-------------------------------------------------|
| window size          | 50 bp | resolution suitable for organelle genomes       |
| `bowtie2 --no-unal`  | on    | drop unmapped reads, keep BAM lean              |
| BAM filter `-F 4`    | on    | retain only mapped reads before sorting         |

## Quality checks

- Overall alignment rate in `align_stats.txt` typically > 90 % for organelle
  reads against a matched assembly
- Coverage in IGV should be relatively uniform across the genome; sharp
  drops or spikes warrant inspection
- GC track typically shows the inverted-repeat structure of plastomes as
  symmetric features
