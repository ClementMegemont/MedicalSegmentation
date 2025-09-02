# Medical Segmentation on ACDC Dataset

## Dataset presentation

This project focuses on developing a Deep Learning model for medical image segmentation.
Specifically, it leverages the Automated Cardiac Diagnosis Challenge (ACDC) dataset, which provides cine-MRI images of the heart. Each case in the dataset is manually annotated by clinical experts, highlighting three key cardiac structures:

- Left Ventricle (LV)
- Right Ventricle (RV)
- Myocardium (Myo)

The goal of this work is to automatically segment these regions of interest, which are critical for assessing cardiac function and detecting potential pathologies.

Below is an example of the dataset, including MRI images alongside their corresponding ground-truth annotations.

<table>
  <tr>
    <td align="center">
      <img src="images//patient006_01_5.png" width="300" /><br>
      MRI Image
    </td>
    <td align="center">
      <img src="images/legend1.png" width="300" /><br>
      Ground Truth
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="images//patient068_01_2.png" width="300" /><br>
      MRI Image
    </td>
    <td align="center">
      <img src="images/legend2.png" width="300" /><br>
      Ground Truth
    </td>
  </tr>
</table>

## Model Architecture

The model used is an enhanced version of the U-Net, called Attention U-Net, which is widely applied in medical image segmentation.
It is structured into four main parts: the encoder, the bottleneck, the decoder, and the final output layer.

1. **Encoder**

The encoder consists of a sequence of convolutional blocks (conv_block), each containing two convolutional layers followed by Batch Normalization and ReLU activation.
After each block, a MaxPooling operation reduces the spatial resolution by half while increasing the number of feature channels (from 16 → 32 → 64 → 128 → 256).
This path progressively captures more complex features while compressing spatial information.

2. **Bottleneck**

At the deepest level, the network has a convolutional block with 256 channels.
This stage captures high-level semantic features and the global context of the image.

3. **Decoder**

The decoder progressively reconstructs the segmentation map. Each stage starts with an upsampling operation (up_conv) that doubles the spatial resolution and reduces the number of channels.
Skip connections from the encoder are integrated, but before concatenation, they pass through an attention block (Attention_block).
These attention gates learn to emphasize relevant regions while suppressing irrelevant information.
After concatenation, a convolutional block refines the combined features.

4. **Output Layer**

Finally, a 1x1 convolution reduces the number of channels to match the desired output (output_ch=1 in this project).
This produces the final segmentation map.

A diagram illustrating the architecture of the Attention U-Net is provided below.

<p align=center>
  <img src="images/model.jpg" width="600"/>
</p>

**Why use Attention U-Net instead of classic U-Net?**

Unlike the standard U-Net, the Attention U-Net introduces attention gates in the skip connections.
These gates filter the encoder features before passing them to the decoder.
They allow the model to focus on anatomically relevant regions (e.g., ventricles, myocardium) while ignoring irrelevant background.
This leads to sharper boundaries and improved robustness in medical image segmentation.

