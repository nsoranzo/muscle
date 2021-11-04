![Muscle5](http://drive5.com/images/muscle5_header.jpg)

# muscle
Multiple sequence alignment generating high-accuracy ensemble of replicates

## Downloads

Binary files are self-contained, no dependencies.

Linux [muscle](https://github.com/rcedgar/muscle/raw/main/binaries/muscle)   
Windows [muscle.exe](https://github.com/rcedgar/muscle/raw/main/binaries/muscle.exe)   

## Usage

    Align FASTA input, write aligned FASTA (AFA) output:
        muscle -align input.fa -output aln.afa

    Align large input using Super5 algorithm if -align is too expensive,
    typically needed with more than a few hundred sequences:
        muscle -super5 input.fa -output aln.afa

    Single replicate alignment:
        muscle -align input.fa -perm PERM -perturb SEED -output aln.afa
        muscle -super5 input.fa -perm PERM -perturb SEED -output aln.afa
            PERM is guide tree permutation none, abc, acb, bca (default none).
            SEED is perturbation seed 0, 1, 2... (default 0 = don't perturb).

    Ensemble of replicate alignments, output in Ensemble FASTA (EFA) format,
    EFA has one aligned FASTA for each replicate with header line "<PERM.SEED":
        muscle -align input.fa -stratified -output stratified_ensemble.efa
        muscle -align input.fa -diversified -output diversified_ensemble.afa

        -replicates N
            Number of replicates, defaults 4, 100, 100 for stratified,
              diversified, resampled. With -stratified there is one
              replicate per guide tree permutation, total is 4 x N.

    Generate resampled ensemble from existing ensemble by sampling columns
    with replacement:
        muscle -resample ensemble.efa -output resampled.efa

        -maxgapfract F
           Maximum fraction of gaps in a column (F=0..1, default 0.5).

        -minconf CC
           Minimum column confidence (CC=0..1, default 0.5).

    If ensemble output filename has @, then one FASTA file is generated
    for each replicate where @ is replaced by perm.s, otherwise all replicates
    are written to one EFA file.

    Calculate disperson of an ensemble:
        muscle -disperse ensemble.efa

    Extract replicate with highest total CC (diversified input recommended):
        muscle -maxcc ensemble.efa -output maxcc.afa

    Extract aligned FASTA files from EFA file:
        muscle -efa_explode ensemble.efa

    Convert FASTA to EFA, input has one filename per line:
        muscle -fa2efa filenames.txt -output ensemble.efa

    Update ensemble by adding two sequences of digits to each replicate, digits
    are column confidence (CC) values, e.g. "73" means CC=0.73, "++" is CC=1.0:
        muscle -addconfseqs ensemble.efa -output ensemble_cc.efa

    Calculate letter confidence (LC) values, -ref specifies the alignment to
    compare against the ensemble (e.g. from -maxcc), output is in aligned
    FASTA format with LC values 0, 1 ... 9 instead of letters:
        muscle -letterconf ensemble.efa -ref aln.afa -output letterconf.afa

        -html aln.html
            Alignment colored by LC in HTML format.

        -jalview aln.features
            Jalview feature file with LC values and colors.

More documentation at:
    [https://drive5.com/muscle](https://drive5.com/muscle)


# Reference
R.C. Edgar (2021) "MUSCLE v5 enables improved estimates of phylogenetic tree confidence by ensemble bootstrapping"    
[https://www.biorxiv.org/content/10.1101/2021.06.20.449169v1.full.pdf](https://www.biorxiv.org/content/10.1101/2021.06.20.449169v1.full.pdf)
