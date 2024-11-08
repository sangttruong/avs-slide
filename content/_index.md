+++
title = "Reliable and Efficient Amortized Model-based Evaluation"
date = "2024-09-23"
outputs = ["Reveal"]

+++
<div style="display: flex; justify-content: space-between; width: 100%;">
  <img src="images/SOM_vert_Web_Color_LG.png" alt="Main Logo" style="height: 70px; margin-left: -150px;"> 
  <img src="images/sail-logo.jpg" alt="Second Logo" style="height: 70px; margin-right: -180px;">
</div>

{{< slide auto-animate="" >}}
### Accelerate Virtual Screening 
### with Amortized Neural Search 
### and Multi-Objective Bayesian Optimization

---
{{< slide auto-animate="" >}}
## 1.Introduction: Problem Set-up of Virtual Screening
  <ul style="font-size: 24px;">
    <li class="fragment">For <strong>a given protein</strong> linked to a certain disease,
    <span class="fragment">we want to select a few small molecules (i.e., ligand)</span>
    <span class="fragment">from a library of millions candidates</span>
    <span class="fragment">such that the selected candidate will have the highest utility in disease treating.</span>
    </li>
  </ul>

  <ul style="font-size: 24px;">
    <li class="fragment">For modern libraries, it is feasible scaling up to billions or even trillions of compounds enhances the reach and impact of virtual screening.
    </li>
  </ul>

<span class="fragment">
    <figure style="display: flex; flex-direction: column; align-items: center; width: 80%; margin-top: 0px; margin-left: 100px">
    <img src="images/vs.png">
</span>

---
{{< slide auto-animate="" >}}
## 1.Introduction: Problem Set-up of Virtual Screening

<div style="display: flex; justify-content: space-between; align-items: flex-start;">
  <div style="width: 50%;">
    <h3 style="font-size: 28px; margin-bottom: 10px;">Virtual screening presents several challenges:</h3>
    <ol style="font-size: 24px; padding-left: 20px;">
      <li class="fragment" data-fragment-index="1"><strong>Multiple, competing objectives</strong> based on unknown, hard-to-quantify expert knowledge.</li>
      <li class="fragment" data-fragment-index="2"><strong>Limited budget</strong> to try all ligands from the library.</li>
      <li class="fragment" data-fragment-index="3">Some objectives (such as binding affinity) are expensive to evaluate even for a single ligand.</li>
    </ol>
  </div>

  <div style="width: 50%;">
    <h3 style="font-size: 28px; margin-bottom: 10px;">Our research aims to address these challenges:</h3>
    <ol style="font-size: 24px; padding-left: 20px;">
      <li class="fragment" data-fragment-index="1">Actively eliciting expert preferences for virtual screening with many objectives.</li>
      <li class="fragment" data-fragment-index="2">Active Virtual Screening.</li>
      <li class="fragment" data-fragment-index="3">Neural Search Engine with diffusion model.</li>
    </ol>
  </div>
</div>


---
{{< slide auto-animate="" >}}
## 2.Eliciting Expert Preferences in Virtual Screening
  <ul style="font-size: 24px;">
    <li class="fragment">Depending on the specific disease and protein, experts have preferences about characteristics of candidate ligands,
    <span class="fragment">trading off various criteria such as synthesizability, affinity, solubility, and side effects.</span>
    </li>
  </ul>

  <ul style="font-size: 24px;">
    <li class="fragment">Some characteristics are “must-have,” such as synthesizability,
    <span class="fragment">while others are “nice-to-have.</span>
    </li>
  </ul>

<span class="fragment">
  <div style="display: flex; justify-content: center; width: 100%; gap: -30px; margin-top: -100px;">
    <figure style="display: flex; flex-direction: column; align-items: center; width: 45%;">
      <img src="images/lig1.png" style="width: 100%;" alt="Aff: -10.11, PSA: 67.66">
      <figcaption style="text-align: left; font-size: 20px; margin-top: -50px;">Affinity: -10.11, PSA: 67.66</figcaption>
    </figure>
    <figure style="display: flex; flex-direction: column; align-items: center; width: 45%;">
      <img src="images/lig2.png" style="width: 100%;" alt="Aff: -10.11, PSA: 67.66">
      <figcaption style="text-align: left; font-size: 20px; margin-top: -50px;">Affinity: -10.11, PSA: 67.66</figcaption>
    </figure>
  </div>
</span>


---
{{< slide auto-animate="" >}}
## 2.Eliciting Expert Preferences in Virtual Screening
- <p style="font-size: 24px;">
  <span class="fragment">These implicit expert knowledge, encoded as preferences over ligands, are valuable to elicit for effective virtual screening.</span> 
  <span class="fragment">We can leverage toolkits from the field of machine learning from human preferences to tackle this challenge.</span>

<span class="fragment">
<table style="width: 80%; margin-top: 20px; border-collapse: collapse; text-align: center; font-size: 18px;">
  <tr>
    <th style="border: 1px solid #ddd; padding: 8px;">First ligand</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Second ligand</th>
    <th style="border: 1px solid #ddd; padding: 8px;">Preference (x1 > x2)</th>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">[-7.81, 114.38, 0.51]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">[-8.12, 116.28, 0.47]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">0</td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">[-10.45, 186.17, 0.29]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">[-8.12, 116.28, 0.47]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">1</td>
  </tr>
  <tr>
    <td style="border: 1px solid #ddd; padding: 8px;">[-6.18, 35.32, 0.83]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">[-8.12, 116.28, 0.47]</td>
    <td style="border: 1px solid #ddd; padding: 8px;">0</td>
  </tr>
</table>

<p style="font-size: 20px; text-align: center; margin-top: 15px;">
  <em>Each ligand is represented by a set of features, such as affinity, polar surface area, QED drug-likeness score</em>
</p>
</span>

---
{{< slide auto-animate="" >}}
## 2.Eliciting Expert Preferences in Virtual Screening
  <p style="font-size: 24px; text-align: left;">Eliciting preference can be viewed as a logistic regression:</p>
  <ul style="font-size: 24px">
    <li class="fragment"> \( p(y \mid x_1, x_2) = \sigma(f(x_1) - f(x_2)) \) where \( \text{sigmoid}(z) = \frac{1}{1 + e^{-z}} \).</li>
  </ul>

  <p class="fragment" style="font-size: 24px; text-align: left;">If we assume utility is a linear function of ligand features, we can use gradient descent to achieve:</p>
  <ul style="font-size: 24px; text-align: left; margin-left: -700px;">
    <li class="fragment">Train accuracy: 0.95</li>
    <li class="fragment">Test accuracy: 0.95</li>
  </ul>

  <p class="fragment" style="font-size: 24px; text-align: left;">Alternatively, using a Gaussian process yields the same results:</p>
  <ul style="font-size: 24px; text-align: left; margin-left: -700px;">
    <li class="fragment">Train accuracy: 0.95</li>
    <li class="fragment">Test accuracy: 0.95</li>
  </ul>

---
{{< slide auto-animate="" >}}
## 2.Eliciting Expert Preferences in Virtual Screening
<ul style="font-size: 24px;">
  <li class="fragment"><strong>Initial Selection</strong>: Randomly select pairs of ligands from a library \( L \) of 7,000 compounds to explore the chemical space.</li>
  <li class="fragment"><strong>Adaptive Selection</strong>: Optimize future selections from \( L \) to reduce oracle queries, improving efficiency and estimating the weight vector \( W \).</li>
  <li class="fragment"><strong>Reward Function</strong>: For utility \( U \) using logistic regression \( U = W \cdot X \), where:
    <ul style="font-size: 22px;">
      <li class="fragment">\( U \): utility score indicating ligand suitability</li>
      <li class="fragment">\( W \): weights representing feature importance</li>
      <li class="fragment">\( X \): ligand feature vector (f(x_1) - f(x_2))</li>
    </ul>
  </li>
</ul>

---
{{< slide auto-animate="" >}}
## 2.Next Steps
  <p style="font-size: 24px; text-align: left;">Working with Chemists to encode expert preference as the objective function for virtual screening.</p>
  <ul style="font-size: 24px;">
    <li class="fragment">Provide a dataset of ligands characterized by properties such as solubility, toxicity, affinity, requesting chemists to choose their preferences.</li>
    <li class="fragment">Use a classification model to determine the optimal weights (W) that reflect chemist preferences.</li>
    <li class="fragment">Perform active virtual screening to iteratively refine the selection of compounds.</li>
  </ul>


---
{{< slide auto-animate="" >}}
## 3. Active Virtual Screening
  <p style="font-size: 24px; text-align: left;">Even with the right trade-off objective elicited from expert, exhaustively screening millions of candidate from the virtual screening library is practically infeasible.</p>
  <p style="font-size: 24px; text-align: left;">To address this problem, we can choose to screen ligand that looks promising, while avoid ligand that are highly certain to be a bad candidate.</p>
  <ul style="font-size: 24px;">
    <li class="fragment">Bayesian Optimization: Efficiently explores high-potential ligands.</li>
    <li class="fragment">Surrogate model (selection model): Gaussian Process, Neural net, Random forest</li>
    <li class="fragment">Acquisition function: UCB, Greedy, Thompson sampling</li>
  </ul>
  <img src="images/avs.png" alt="Active Virtual Screening Diagram" style="width: 80%;" class="fragment">

---
{{< slide auto-animate="" >}}
## 4. Neural Search Engine
<div style="text-align: left; font-size: 28px; margin-left: 30px;">
  <span class="fragment">
  <p>Traditional physics-based docking tools (e.g., Glide, Smina) are computationally expensive to evaluate ligand affinity.</p>
  </span>
  <ul style="margin-left: 40px;">
    <li class="fragment"> <strong>Traditional Tools</strong>: 15mins/1 pose</li>
    <li class="fragment"> <strong>Our model</strong>: 5s/64 poses</li>
    <li class="fragment"> <strong>Chai</strong>: 1 min/5 poses</li>
  </ul>
</div>

<div style="text-align: left; font-size: 28px; margin-left: 30px;">
  <span class="fragment">
  <p>We leverage the similarity in ligand structure to accelerate the binding by training a diffusion model for molecular docking</p>
  </span>
  <ul style="margin-left: 40px;">
    <li class="fragment"> <strong>Neural Engine</strong>: Uses diffusion model for rapid docking</li>
    <li class="fragment"> <strong>Utility Scoring</strong>: Assesses ligands on affinity, solubility, and toxicity, etc</li>
  </ul>
</div>

---
{{< slide auto-animate="" >}}
## 4. Neural Search Engine
<div style="text-align: left; font-size: 28px; margin-left: 30px;">
  <span class="fragment">
  <p>Accelerate pose search further:</p>
  </span>
  <ul style="font-size: 24px; margin-left: 40px;">
    <li class="fragment"> Local vs blind docking</li>
    <li class="fragment"> Blind docking first during initialization, local docking during active screening process</li>
    <li class="fragment"> Obtain centroid positions through ligand initializations for faster docking time</li>
  </ul>
</div>

<span class="fragment">
  <figure style="display: flex; flex-direction: column; align-items: center;">
    <div style="display: flex; justify-content: center; width: 100%;">
        <img src="images/blind_docking.gif" style="width: 60%; margin-right: 10px;" alt="Blind Docking">
        <img src="images/local_docking.gif" style="width: 60%;" alt="Local Docking">
    </div>
    <figcaption style="text-align: center; font-size: 20px; margin-top: 10px;">Blind Docking (left) vs. Local Docking (right)</figcaption>
  </figure>
</span>


---
{{< slide auto-animate="" >}}
## 5. Diffusion Model: Data collection
<div style="text-align: left; font-size: 24px; margin-left: 30px;"> 
    <span class="fragment"> 
        <p><strong>Data Augmentation Techniques:</strong></p> 
        <ul style="margin-left: 40px; font-size: 20px"> 
            <li class="fragment"><strong>Molecular Dynamics (MD) Augmentation:</strong> Employed 59,330 dynamic frames of 14,387 protein-ligand complexes to model ligand flexibility, amounting to 75K training data.</li> 
            <li class="fragment"><strong>Data Crawling:</strong> Curated 322K protein-ligand complexes from the PDBScan22 dataset, after filtering out unnatural ligands and problematic poses, to enhance structural diversity.</li> 
            <li class="fragment"><strong>Pharmacophore Alignment:</strong> Generated up to 11M pharmacophore-consistent ligand pairs from the Papyrus dataset, significantly expanding the model's training data.</li> 
        </ul> 
    </span> 
    <span class="fragment">
        <div style="display: flex; justify-content: space-between; margin-top: 20px;">
            <div style="width: 49%;">
                <img src="images/md.png" alt="MD Simulation Example" style="width: 100%; height: auto;">
                <p style="text-align: center; font-size: 20px">Figure 1: MD Simulation Trajectories</p>
            </div>
            <div style="width: 49%;">
                <img src="images/pharmacophore.png" alt="Pharmacophore Model Example" style="width: 100%; height: auto;">
                <p style="text-align: center; font-size: 20px">Figure 2: Pharmacophore Modeling</p>
            </div>
        </div>
    </span>
</div>



---
{{< slide auto-animate="" >}}
## 5. Diffusion Model: Training
<div style="text-align: left; font-size: 24px; margin-left: 30px;"> 
    <span class="fragment"> <p>Diffusion models are probabilistic generative models that transform data from a noisy distribution into a structured, meaningful output through a reverse process.</p> </span> 
    <span class="fragment"> <p>Applying SDEs for controlled generative modeling:</p> </span> 
    <ul style="margin-left: 40px;"> 
        <li class="fragment"> Initiate with Gaussian noise on ligand coordinates</li> 
        <li class="fragment"> Employ Euler-Maruyama to denoise step-by-step: <ul> 
        <li style="font-size: 20px;">\(X_{t-\Delta t} = X_t + f(X_t, t) \Delta t + g(t) \sqrt{\Delta t} \cdot \epsilon\)</li> </ul> </li> 
    <li class="fragment"> Optimize ligand structure discovery through iterative sampling steps via Euler</li> 
    </ul> 
</div>

---
{{< slide auto-animate="" >}}
## 5. Diffusion Model: Results
<div style="text-align: left; font-size: 24px; margin-left: 30px;"> <span class="fragment"> <p><strong>Benchmark on Posebusters Dataset:</strong></p> </span> <p class="fragment">Posebusters Version 1 includes 428 protein-ligand structures, and Version 2 has 308, all released to the PDB after 2021. Performance is measured by the percentage of protein-ligand pairs with pocket-aligned ligand RMSD under 2 Å.</p> </div>

<span class="fragment">
  <figure style="display: flex; flex-direction: column; align-items: center;">
    <div style="display: flex; justify-content: center; width: 100%; gap: 10px; margin-top: -20px;">
        <img src="images/docking_results.png" style="width: 100%; max-width: 1000px;">
    </div>
  </figure>
</span>


---
{{< slide auto-animate="" >}}
## 6. Contrained settings

<div style="text-align: left; font-size: 24px; margin-left: 10px;">
  <span class="fragment">
  <p>This graph showcases the outcome of a PCA-based virtual screening process to identify and compare ligands based on their chemical similarity.</p>
  </span>
  <ul style="text-align: left; font-size: 20px; margin-left: 40px;">
    <li class="fragment">PC1 and PC2: represent the principal components derived from PCA, summarizing complex ligand properties into a comprehensible two-dimensional space.</li>
    <li class="fragment">Similarity Metric: using the Tanimoto coefficient, comparing chemical fingerprints.</li>
  </ul>
</div>

<span class="fragment">
  <figure style="display: flex; flex-direction: column; align-items: center;">
    <div style="display: flex; justify-content: center; width: 95%;">
        <img src="images/similarity.png" style="width: 50%;" alt="Virtual Screening with Multiple Query Ligands and Constrained Similarity">
    </div>
    <figcaption style="text-align: center; font-size: 20px; margin-top: 10px;">Virtual Screening with Multiple Query Ligands and Constrained Similarity</figcaption>
  </figure>
</span>


---
{{< slide auto-animate="" >}}
## 7. Putting it all together
<div style="text-align: left; font-size: 24px; margin-left: 0px;">
  <p>
    <span class="fragment"> We perform active virtual screening on the inferred expert utility function.</span>
    <span class="fragment"> Our procedure respects expert preference (both hard and soft constraint) and probabilistic model to come up with a good candidate set.</span>
    <span class="fragment"> To search for poses required for objectives such as affinity, we accelerate the pose search by a neural search engine.</span>
  </p>
</div>

<div style="text-align: left; font-size: 24px; margin-top: 20px;">
  <span class="fragment"><strong>Metrics for evaluation</strong></span>
</div>

<div style="text-align: left; font-size: 24px; margin-left: 10px;">
  <span class="fragment">
  <p><strong>Regret</strong></p>
  </span>
  <ul style="margin-left: 40px;">
    <li class="fragment"><strong>Definition</strong>: Difference in affinity between the best possible ligand and the top ligand found by the model within the top_k \%\.</li>
    <li class="fragment">\[ \text{Regret} = A_{\text{best}} - A_{\text{model}} \]</li>
  </ul>
</div>

<div style="text-align: left; font-size: 24px; margin-left: 10px;">
  <span class="fragment">
  <p><strong>Percent of Best Ligand Found</strong></p>
  </span>
  <ul style="margin-left: 40px;">
    <li class="fragment"><strong>Definition</strong>: Percentage of screened ligands close in affinity to the best possible ligand. (top_k \%\)</li>
  </ul>
</div>

---
{{< slide auto-animate="" >}}
## 7. Putting it all together: Screening Results
<span class="fragment">
  <figure style="display: flex; flex-direction: column; align-items: center;">
    <div style="display: flex; justify-content: center; width: 100%; gap: 70px; margin-top: -30px;">
        <img src="images/percent.png" style="width: 40%; max-width: 450px;">
        <img src="images/regret.png" style="width: 40%; max-width: 450px;">
    </div>
  </figure>
</span>


---
{{< slide auto-animate="" >}}
## 7. Putting it all together: Screening Results
<span class="fragment">
  <figure style="display: flex; flex-direction: column; align-items: center;">
    <div style="display: flex; justify-content: center; width: 100%; gap: 10px; margin-top: -20px;">
        <img src="images/time.png" style="width: 70%; max-width: 650px;">
    </div>
  </figure>
</span>

---
{{< slide auto-animate="" >}}
## 7. Putting it all together: Next steps
  <ul style="margin-left: 40px;">
    <li class="fragment"> Run virtual screening on bigger library (100k, 1M) compounds</li>
    <li class="fragment"> Improve on performance of diffusion model</li>
  </ul>