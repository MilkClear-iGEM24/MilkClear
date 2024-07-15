---
title: "Wiki"
author: "Thanh Le"
date: "2024-07-11"
output: html_document
---

**Note: The documentation may include redundant details for the purpose of understanding the bioinformatics analyses. Bold means requiring validation of info.**

# Structural bioinformatics

## ERRy
Among the choices of biosensors for BPA, we choose ERRy due to its high binding affinity to BPA, compared to those of endogenous ligands including ____E2, …_____

Firstly, our concern is whether the substitution of hERa for ERRy LBD changes the performance of the system. To investigate this question, we superimpose the structures of hERa with ERRy, and color the residues by RMSD.
Retrieving the sequences of ERRy and hERa from PDB files 2GPO and 5WGD, we aligned the 2 sequences globally using the Needleman-Wunsch algorithm. As 2 sequences are highly similar as indicated by 33.1% sequence identity, we proceed to do alignment using the align command in Pymol.
2GPO is the crystal structure of unliganded ERRy LBD with RIP140 coactivator [Wang, et. al, 2006](https://www.jbc.org/article/S0021-9258(20)71951-4/fulltext). Here, we see ERRy adopts an agonist conformation with H12 covering the ligand binding pocket, creating an interacting surface for the NR-box (or LXXLL motif) containing coactivator, RIP140. We choose 5WGD as an analogous structure of ER-alpha, as ERa is also in its transcriptionally active conformation here, via binding to SRC2 and an agonist. The 2 structures are highly similar, with r.m.s.d = 0.735 A over 1067 atoms, while the most deviated region being the N-terminus end of helix 10.

![Figure 1: ERRy LBD in complex with coactivator RIP140 (PDB: 2GPO) superimposed onto agonist-bound hERa, complexed with coactivator SRC-2. Red indicates high r.m.s.d, while blue indicates low r.m.s.d.](results/2gpoa_5wgda_rmsdcol.png)

As shown in the first crystal structure of antagonist-bound ER LBD, resolved by [Brzozowski, et. al (1997)](https://www-nature-com.ep.fjernadgang.kb.dk/articles/39645), the dimerization interface of ER involves the zipper-like interactions between helices 11, and other contacts among helices 8, 9, and 10. Hence, the difference in helix 10 may affect the dimerization of ERRy, which is a pre-requisite for the binding of ERRy to the estrogen-responsive element (ERE) allowing transcription. We illustrated the dimerization interface of ERRy below. 
However, as we use a different **co-activator, VP64, to recruit different set of transcriptional machinery to a CYC1 promoter** instead of ERE, we speculate that this structural difference has a trivial impact on the performance of our system.

As demonstrated in [Greschik et. al, 2002](https://www.cell.com/AJHG/fulltext/S1097-2765(02)00444-6) study, ERRy-LBD without ligand superimposes well with E2-bound ERa with a root-mean-square deviation of only 1.05A. Furthermore, mutants with filled-up ligand binding cavity is still able to interact with co-activators SRC-1 and are transcriptionally active. Hence, to increase the sensitivity of our system for BPA, 2 objectives are increasing the binding affinity of ERRy to BPA and minimizing the basal transcriptional activity of the chimeric activator.

### Minimize leaky transcription:

[Need results from wetlab here to confirm if there's false positive. Mention a bit about how the synthetic biology method can be used, via introducing a repressor, to minimize leaky transcription.]

To minimize basal transcription of the chimeric activator, we aim to understand the conformational changes by ERRy upon binding to BPA that dissociates itself from HSP90 to derive at our objectives for engineering ERRy structure. Our analyses are faced with 2 major roadblocks: 1. lack of existing model and evidence of HSP90-ERRy interaction 2. lack of understanding on conformational changes of ERRy upon binding to BPA.

#### Creating a model for HSP90-ERRy complex

To address the first question, we set out to create a model of HSP90-ERRy complex, using existing model from HSP90-GR (glucocorticoid receptor)-p23. With p23 stabilizing the closed clamped substrate-bound conformation of HSP90, 

While there are evidences for protein-protein interactions of HSP90 with some steroid hormone receptors including ERa and GR, interaction between HSP90 and ERRy hasn't been reported yet. However, through superposing the structures of ERRy with GR and with ERa, we suspect that HSP90 interacts in the same manner, due to the conservation in the interacting domains of ERRy and GR, as demonstrated below.

[Insert image of superposition colored by conservation <= Jalmol? refer to figure 2b in Noddings et. al 2021 Structure of Hsp90–p23–GR reveals the Hsp90 client-remodelling mechanism]

[A brief description of what chains of GR (the 3 interfaces) are important in interacting with HSP90]

Therefore, we proceed to dock ERRy onto HSP90 using [HDOCK](HDOCK Server (hust.edu.cn).



To understand the conformational changes between 3 states of ERRy - unliganded, HSP90-bound, and BPA-bound, we conducted normal mode analysis using [elNemo](https://www.sciences.univ-nantes.fr/elnemo/). For the structure of HSP90-bound ERRy, as there's no existing model, we created a new model of HSP90-ERRy complex based on HSP90-GR-p23 complex. 

#### Computational methods for investigating conformational changes of ERRy

While the existing structures of unliganded and liganded ERRy have been deposited in RCSB, these represent static representations of ERRy, which provide an incomplete view of the full scope of ERRy structure - activity relationship. Hence, we tried out 2 methods to simulate the conformational changes of ERRy among 3 states - unliganded, BPA-bound, and HSP90-bound.

##### Molecular Dynamics Simulation

##### Normal mode analysis and Elastic Net Model

Previous structural studies (**citations**) showed that protein conformational change can be described through a handful of protein motions at minimum energy level. These, normal modes are orthogonal to each other, meaning that the conformational change can be described by superimposing all normal modes.

NMA starts with a potential energy surface of a protein, i.e. dependent variable being the potential V, and independent variables being the components of the system (the coordinates). Then a Hessian matrix is calculated from the potential energy function, containing all the second derivatives of V against every components in the system. Referring to equation 2 in [Bauer and Bauerová-Hlinková (2020)](https://www.intechopen.com/chapters/73720), i and j are like x and y axes, and if the protein has 100 atoms, the Hessian matrix will have the size of 100x100. If we describe each atom in 3D - x,y,z, the size of the Hessian matrix will be 300x300 (change in V against xy, yz, and xz) then.

By doing eigen-decomposition on this Hessian matrix, we will retrieve eigenvectors as normal mode vectors, describing ONLY the direction and the displacement of each particle relative to each other. We also receive the eigenvalues, which is the squares of the vibration frequency of a particular mode. The lower the frequency of a mode, the larger the amplitude of the displacement, often indicating large-scale conformational changes and involvement of larg number of atoms. Hence, elNemo only displays the top 5 normal modes with lowest frequencies.

The Elastic Net Model (ENM) is a modification to conduct NMA in a less computationally expensive way. Instead of using the potential energy function derived from a force field - equation 1 [Bauer and Bauerová-Hlinková (2020)](https://www.intechopen.com/chapters/73720), ENM uses the Hooke potential, based on a simplified model of a protein structure where atoms are connected by harmonic springs. 




### Increase binding affinity of ERRy to BPA




[A short description of normal mode analysis]

## Normal mode analysis





Understand why ERRy doesn’t bind to E2 & how ERRy binds to BPA => conduct virtual screening of the new binding pocket



To model the binding of transcription factor to our promoter, we use [Haddock](https://link.springer.com/article/10.1007/s10858-013-9734-x)


