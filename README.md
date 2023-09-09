# Handling--ImbalancedData-WBCE-BCE
Discover how WBCE, intended for data balance, fell short in our performance evaluation. Explore insights and code for imbalanced datasets, based on Helium Bubble Detection research. This recent publication, authored by me, serves as the basis for the explanation and figures below:: (Application of a deep learning semantic segmentation model to helium bubbles and voids in nuclear materials S Agarwal, A Sawant, M Faisal, SE Copp, J Reyes-Zacarias, YR Lin, SJ Zinkle, Engineering Applications of Artificial Intelligence 126, 106747)


  ![Fig. 2](https://github.com/shradhautk/Handling--ImbalancedData-WBCE-BCE/assets/101154495/ed4eca65-b56f-4d17-880d-a63bc1008a41):Fig. 2. Image segmentation results using different loss functions (a) TEM image of the helium voids (b) Ground truth labels done by humans (c) Colored overlap of  DefectSegNet’s pixel-wise semantic segmentation prediction (or mask) using WBCE loss function with GT; dark blue is false positive, yellow is false negative, and teal is where prediction matches with mask (d) Colored overlap of DefectSegNet’s pixel-wise semantic segmentation prediction using Distance map loss function with GT; dark blue is false positive, yellow is false negative, and teal is where prediction matches with mask (e) The overlap of DefectSegNet’s prediction masks using WBCE loss function with the TEM image (f) The overlap of DefectSegNet’s prediction masks using Distance map loss function with the TEM image.

![Fig. 3](https://github.com/shradhautk/Handling--ImbalancedData-WBCE-BCE/assets/101154495/16f1ec2a-b1ea-4875-853f-a0bece124c98):Fig. 3. Image segmentation results using different loss functions (a) TEM image of the helium voids (b) Ground truth labels done by humans (c) Colored overlap of  DefectSegNet’s pixel-wise semantic segmentation prediction (or mask) using WBCE loss function with GT; dark blue is false positive, yellow is false negative, and teal is where prediction matches with mask (d) Colored overlap of DefectSegNet’s pixel-wise semantic segmentation prediction using Distance map loss function with GT; dark blue is false positive, yellow is false negative, and teal is where prediction matches with mask (e) The overlap of DefectSegNet’s prediction masks using WBCE loss function with the TEM image (f) The overlap of DefectSegNet’s prediction masks using Distance map loss function with the TEM image. 



Introduction

Utilization of the DefectSegNet Model

In this research, we employed the DefectSegNet model [14] for the purpose of semantic segmentation. This model is grounded in the U-Net and DenseNet image segmentation architectures.

Challenges with the WBCE Loss Function

However, it came to our attention that the utilization of the WBCE (Weighted Binary Cross Entropy) loss function, as originally proposed in [14], did not yield satisfactory results, particularly concerning the qualitative aspects of segmentation accuracy.

Visual Representation of the Issue

To expound on this matter, we have included Figure 2 and Figure 3 herein, as well as supplementary figures. These visual representations portray the semantic segmentation predictions derived from both the distance map loss function and WBCE. The overlay of these segmentation masks on the ground truth (GT) images and TEM images serves to underscore the disparities, with a particular focus on the boundaries.

Notable Disparities in Segmentation Quality

It is evident that Figure 2(c) and Figure 3(c) exhibit more conspicuous areas denoted in dark blue, signifying false negatives, especially along the boundaries, when contrasted with Figure 2(d) and Figure 3(d). To underscore this distinction, we have demarcated specific areas in Figure 2(c) and 2(d) where this discrepancy is most pronounced. Furthermore, we have highlighted an artifact that appears more extended in Figure 3(c) compared to Figure 3(d). On a broader note, artifacts segmented utilizing the WBCE loss function manifest a greater breadth in contrast to those segmented using the distance map loss function. This observation alludes to a bias towards white and an inclination towards overprediction.

Comparison of Loss Functions

In the preceding sections, we elucidated the intricacies of the Binary Cross Entropy (BCE) loss and the Weighted Binary Cross Entropy (WBCE) loss functions. These functions are designed to gauge the variance between actual and predicted image masks, considering class predictions and ground truth at individual pixel levels. The mathematical formulation for these functions is as follows:

BCE (Binary Cross Entropy) Loss:
CE(p, p ̂) = -(p log(p ̂) + (1 - p) log(1 - p ̂))

WBCE (Weighted Binary Cross Entropy) Loss:
WBCE(p, p ̂) = -(βp log(p ̂) + (1 - p) log(1 - p ̂))

Addressing the Issue

Within our context, it becomes apparent that the WBCE loss function, instead of achieving an equilibrium between negative and white pixels, should place a more pronounced emphasis on the equitable treatment of white pixels within the dataset. However, it appears to be biased towards white and results in an undue tendency towards overprediction. To ameliorate this predicament, we have embraced a loss function that finds common application in the realm of biomedical image analysis. This loss function, referred to as the Distance Map Loss (DML), assigns higher weights to pixels situated in closer proximity to object boundaries, thus lending more weight to pixels in these regions. Following the formulation by Calivia et al., the Distance Map Loss (DML) for a given pixel is defined as:

DML = (1 + ϕ) * BCE

In this equation, BCE signifies the binary cross-entropy loss, while ϕ derives from the normalized distance map of the image. The value of ϕ assumes a value of 1 at object boundaries (void) and 0 at interior points. It is noteworthy that both DML and WBCE employ the BCE loss function as their foundation but employ coefficients, such as β and ϕ, in distinct mathematical arrangements.

Superior Performance of the Distance Map Loss (DML)

As clearly demonstrated through the visual representations in Figure 2 and Figure 3, the Distance Map Loss (DML) has exhibited superior qualitative performance compared to WBCE. Overall, it has proven to enhance segmentation quality within the scope of our study.




