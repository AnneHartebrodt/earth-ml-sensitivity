# Identify mutant thermostable nanobodies with conserved function

We showcase the applicability of ML models for a potential real-world application. The question we ask is: "Can we identify alternative nanobodies that (1) are more thermostable than the wildtype and (2) preserve the wildtype's function and structure?"

### Table of contents

- [Motivation](#motivation)
- [Predict mutant nanobodies with conserved function using nanoBert](#step1)
- [Predict the 3D structure of the wildtype and mutant nanobodies using AlphaFold2](#step2)
- [Predict  melting temperature for the mutant nanobodies using TEMPRO](#step3)
- [Select putative thermostable mutant nanobodies](#step4)


## Motivation <a name="motivation"></a>

Finding thermostable nanobodies is important because they offer enhanced stability under extreme conditions, making them more suitable for a variety of real-world applications, including:

#### Improved Shelf Life and Storage:

Thermostable nanobodies can be stored and transported without refrigeration, crucial for global distribution, especially in regions with limited cold-chain infrastructure. This is particularly beneficial for healthcare in underdeveloped areas.

#### Enhanced Performance in Diagnostics and Therapeutics:

In diagnostic tools, thermostable nanobodies maintain functionality over a wider range of temperatures, ensuring reliability in point-of-care settings. In therapeutics, they remain effective in the human body under physiological stress, improving drug stability.

#### Industrial Applications:

Thermostable nanobodies are valuable in harsh industrial environments, such as in biotechnology and pharmaceutical manufacturing, where high temperatures are required for certain processes.

## 1. Predict mutant nanobodies with conserved function using nanoBert <a name="step1"></a>

We uses a collection of `567 nanobodies` to predict alternative/mutated nanobodies, which are predicted to be functional similar to the wildtype. We used for this, `nanoBERT`, which is a nanobody-specific transformer. Its primary application is positing infilling, predicting what amino acids could be available at a given position according to the nanobody-specific distribution.  

We extracted for every nanobody the sequence of the `CDR3` loop and predicted functional related amino-acids for each position in the CDR3 sequence. We kept only the predictions with a `probability > 0.5`, which resulted in more than 700 alternative nanobodies. In average one mutant nanobody was predicted per wildtype nanobody with `probability > 0.5`.

![](./scripts/figures/7_numberofalternative_nanobodies.png)

Our analysis of the functional predictions for amino-acid exchanges reveals that the predicted mutant nanobodies frequently exhibit hydrophobic substitutions. This observation aligns with the fact that the flanking regions of the CDR3 loops are inherently hydrophobic. The most frequent exchange is V (Val) > A (Ala).

![](./scripts/figures/8_mutated_aa_nanobodies.png)

## 2. Predict the 3D structure of the wildtype and mutant nanobodies using AlphaFold2 <a name="step2"></a>

For a selection of mutated and wildtype nanobodies we performed 3D structure prediction using AlphaFold2 (https://neurosnap.ai/service/AlphaFold2).

## 3. Predict Tm for the mutant nanobodies using TEMPRO <a name="step3"></a>

We predicted the melting temperature for the 700+ alternative nanobodies using the TEMPRO prediction model.

## 4. Select putative thermostable mutant nanobodies <a name="step4"></a>

We are interested to select mutant nanobodies candidates which are more thermostable compared to wildtype, i.e. the melting temperature of the mutant(Tm_mut) is higher compared to the wildtype (Tm_wt). 

We first look at the temperature changes between Tm_wt and Tm_mut accross all predictions and observe that many of the predicted alternative nanobodies have lower predicted melting temperature. 

![](./scripts/figures/10_melting_temperature_all.png)

![](./scripts/figures/11_melting_temperature_all_colored_by_aa.png)

For this purpose we keep in our dataset predictions which have:

- keep only entries for the Tm_wt was predicted accurately (close to experimental Tm)
- mutation probability > 0.5 according to nanoBERT
- BLOSUM80 for the aa exchange > -1 
- Tm_mut - Tm_wt > 3 degrees

Source: `scripts/3_Predict_Thermostable_MutantNanobodies.ipynb`.

