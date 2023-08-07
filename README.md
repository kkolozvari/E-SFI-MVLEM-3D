# E-SFI-MVLEM-3D
Efficient, three-dimensional, four-node, macroscopic element for RC walls with shear-flexural interaction

[K. Kolozvari](mailto:kkolozvari@fullerton.edu), CSU Fullerton<br/>
C. N. Lopez, University of Chile, Santiago<br/>
L. M. Massone, University of Chile, Santiago<br/>

## Description

The E-SFI-MVLEM-3D model (Figure 1a) is a three-dimensional four-node element with 24 DOFs that incorporates axial-flexural-shear interaction and can be used for nonlinear analysis of non-rectangular reinforced concrete walls subjected to multidirectional loading. The E-SFI-MVLEM-3D model is derived by combining two previously available models, a two-dimensional ([E-SFI](https://github.com/carloslopezolea/E-SFI_Documentation)) model, and a three-dimensional ([SFI-MVLEM-3D](https://kkolozvari.github.io/SFI-MVLEM-3D/)) model. The major enhancement in the model formulation compared to its parent SFI-MVLEM-3D comes from implementing a closed-form solution for calculating horizontal axial strains at fibers of the wall element. This significantly reduced the number of element degrees of freedom, which resulted in analysis run-time that is reduced to approximately 25% and a convergence rate that is increased roughly two times (Figure 2).

![Model_Formulation: General](https://user-images.githubusercontent.com/53920372/110258567-14569400-7f58-11eb-9e57-f367640ed881.JPG)<br/>
**Figure 1: E-SFI-MVLEM-3D Element Formulation**

![Fig 2](https://github.com/kkolozvari/E-SFI-MVLEM-3D/assets/53920372/c3ce898e-fd22-49a7-b79c-c52e438b4d4a)<br/>
**Figure 2: E-SFI-MVLEM-3D Element Formulation: In-plane behavior**

### E-SFI-MVLEM-3D Input
```markdown
element E-SFI_MVLEM_3D eleTag iNode jNode kNode lNode m  -thick {Thicknesses} -width {Widths} -mat {Material_tags} 
<-CoR c> <-ThickMod tMod> <-Poisson Nu>  <-Density Dens>
```

| parameter | description |
|----------|------------|
| eleTag | unique element tag|
| iNode jNode kNode lNode | tags of element nodes defined in counterclockwise direction|
| m | number of element macro-fibers|
| {Thicknesses} | array of m macro-fiber thicknesses|
| {Widths} | array of m macro-fiber widths |
| {Material_tags}| array of m macro-fiber nDMaterial [FSAM](https://opensees.berkeley.edu/wiki/index.php/FSAM_-_2D_RC_Panel_Constitutive_Behavior) tags|
| c | location of the center of rotation from the base (optional, default = 0.4 (recommended))|
| tMod	| thickness modifier for out-of-plane bending behavior (optional; default = 0.63, which is equivalent to 25% of uncracked stiffness)|
| Nu | Poisson ratio for out-of-plane bending (optional, default = 0.25)|
| Dens | element density (optional, default = 0.00)|

### Recorders

The following recorders are available with the SFI-MVLEM-3D element.

| recorder | description |
|----------|------------|
| globalForce | Element global forces|
| Curvature | Element curvature|
| ShearDef | Element shear deformation|
| RCPanel $fibTag $Response | Returns RC panel (macro-fiber) $Response for a $fibTag-th panel (1 ≤ fibTag ≤ m). For available $Response-s refer to nDMaterial [FSAM](https://opensees.berkeley.edu/wiki/index.php/FSAM_-_2D_RC_Panel_Constitutive_Behavior) |

## Example

Specimens SD-06-00/45 (Habasaki et el., 1999) is analyzed using the E-SFI-MVLEM-3D. Figure 3 shows the E-SFI-MVLEM-3D model of specimens SD-06-00/45.

![Fig 3](https://github.com/kkolozvari/E-SFI-MVLEM-3D/assets/53920372/1c59d721-d5fa-4c83-99d1-113743ebf208)<br/>
**Figure 3: E-SFI-MVLEM-3D Model of specimens SD-06-00/45**

Figure 4 shows the comparison of experimental and analytical load-deformation responses of specimens SD-06-00 and SD-06-45 obtained using the proposed E-SFI-MVLEM-3D element. Figure 4a illustrates that the E-SFI-MVLEM-3D model can predict with very good accuracy the initial stiffness of the specimen as well as its stiffness under cyclic loading. Peak lateral strength is underestimated by approximately 5%, while the pinched shape of hysteretic loops, specific for shear-dominated wall behavior, is well-predicted by the model. Figure 4b further demonstrates that the proposed E-SFI-MVLEM-3D can accurately capture the behavior of the squat nonplanar walls under cyclic loading even in a direction that is not parallel to the specimen principal axes (specimen SD-06-45). Stiffness, although slightly overestimated, is predicted well, as well as the overall characteristics of the specimen hysteretic behavior (e.g., pinching, reloading/unloading stiffness). The lateral load capacity of the specimen is also well-predicted by the model and is within 5% of the experimentally measured value. For both specimens, the strength in the proposed model is controlled by large tensile diagonal strains at the bottom of the specimen, where all vertical bars are yielding, associated with shear failure. Results presented in Figure 4 demonstrate that the proposed modeling approach is suitable for simulation of the behavior of nonplanar, shear-dominated wall specimens as its formulation allows the prediction of nonlinear shear formulations in a mechanical manner. 

![Fig 4](https://github.com/kkolozvari/E-SFI-MVLEM-3D/assets/53920372/69d94744-7fce-4b2b-99b5-5e152073bbe9)<br/>
**Figure 4: Experimental vs. E-SFI-MVLEM-3D load-deformation responses**

Finally, Figure 5 compares the contributions of shear deformations at various deformation levels obtained from the analysis of the two considered specimens against the contributions recorded during the experiment. Experimental shear deformations are obtained using data measured with diagonal sensors, while analytical shear deformations are calculated as cumulative (over the height of the wall specimen) shear deformations recorded at a height of c×h (Figure 3a) of each element. Note that shear deformations at 45° for specimen SD-06-45 were obtained in the model by projecting the shear deformations (the sum of the shear deformations over the height) of two orthogonal faces of the wall specimen. Per Habasaki et al. (1999), contributions of shear deformations to the total lateral displacement of the specimens were between 80-85% for both specimens. As can be observed from Figure 12, analytically predicted contributions of shear deformations generally vary between 80% and 90%, with average contribution across all displacement levels of 83.5% and 85.0% for specimen SD-06-00 and SD-06-45, respectively, which is in excellent agreement with experimental data. This observation suggests that the formulation of the E-SFI-MVLEM-3D can capture with very good accuracy nonlinear shear deformations in nonplanar walls under cyclic loading.

![Fig 5](https://github.com/kkolozvari/E-SFI-MVLEM-3D/assets/53920372/a53f17cf-d6f8-47e4-bf90-7af25d5d8888)<br/>
**Figure 5: Contributions of shear deformations to total lateral displacements for specimen**

## References
Kristijan Kolozvari, Carlos N. López, Leonardo M. Massone (2023), "Efficient Three-dimensional Shear-flexure Interaction Model for Reinforced Concrete Walls", Engineering Structures, xx, xxxxxx. [link](https://www.sciencedirect.com/science/article/pii/S2352710221008044)
