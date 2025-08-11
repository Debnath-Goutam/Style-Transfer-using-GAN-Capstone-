# Style Transfer Using CycleGAN

## Overview
This project implements a Cycle-Consistent Generative Adversarial Network (**CycleGAN**) to translate between **T1-weighted** and **T2-weighted** brain MRI scans.  
The primary goal is to learn the mapping between these two modalities without requiring paired images, enabling cross-modality synthesis for medical imaging research and augmentation.

## Objectives
- Train a CycleGAN model to perform **T1 → T2** and **T2 → T1** MRI translation.
- Preserve anatomical structure while altering MRI contrast patterns.
- Evaluate visual quality and domain adaptation effectiveness.

## Dataset
- **Source**: MRI scans from the project’s provided dataset (T1 and T2 in PNG format).  
- **Structure**:  
  ```
  TrainT1/   # T1-weighted MRI images
  TrainT2/   # T2-weighted MRI images
  ```
- Images are preprocessed:
  - Resized to **256×256**
  - Converted to grayscale (single channel)
  - Normalized to the range [-1, 1] for GAN stability

## Methodology
### Model
- **Generator**: U-Net style encoder–decoder with residual blocks for domain translation.
- **Discriminator**: PatchGAN classifier for local realism checks.
- **Losses**:
  - **Adversarial Loss** – Encourages domain-specific realism.
  - **Cycle Consistency Loss** – Preserves content during forward–backward translation.
  - **Identity Loss** – Maintains color/intensity profiles for same-domain inputs.

### Training
- **Batch Size**: 1
- **Epochs**: 100
- **Optimizer**: Adam (β₁=0.5, β₂=0.999)
- **Loss Weights**:  
  - Cycle Consistency: 10  
  - Identity Loss: 1.0  
  - (Optional) Perceptual Loss: 0.01  

## Results
### T1 → T2 Translation
- CSF regions brightened as expected.
- Gray–white matter contrast partially inverted.
- Some blurring and loss of cortical detail.

### T2 → T1 Translation
- CSF darkened, matching T1 appearance.
- White matter brighter than gray matter.
- Notable checkerboard artifacts in some regions.

**Sample Output:**
| T1 Input | T1→T2 Output | T2 Input | T2→T1 Output |
|----------|-------------|----------|-------------|
| ![T1](sample_t1.png) | ![T2_generated](sample_t1to2.png) | ![T2](sample_t2.png) | ![T1_generated](sample_t2to1.png) |

## Limitations
- Generated scans sometimes lose fine anatomical structures.
- Checkerboard patterns and texture artifacts appear in certain outputs.
- Contrast transformations are not always fully realistic.

## Future Improvements
- Replace transpose convolutions with upsampling + convolution to reduce artifacts.
- Increase identity loss weight to better preserve structure.
- Incorporate perceptual and edge-preserving losses.
- Extend training to 200+ epochs with larger batch size.
- Apply histogram matching during preprocessing.

## How to Run
1. **Install dependencies**:
   ```bash
   pip install tensorflow numpy matplotlib scikit-learn tqdm
   ```
2. **Organize dataset**:
   ```
   TrainT1/
   TrainT2/
   ```
3. **Run training**:
   ```bash
   python train.py
   ```
4. **Generate translations**:
   ```bash
   python test.py
   ```

## Acknowledgments
- Zhu et al., *Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks* (CycleGAN)
- MRI dataset providers and preprocessing scripts.

```python

```
