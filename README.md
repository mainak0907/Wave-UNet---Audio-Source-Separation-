# Wave-UNet---Audio-Source-Separation-

Trained Model - https://drive.google.com/file/d/1qJEu43ibFPX5OxDfdDvzMjiD244J9-QP/view?usp=sharing

# Wave-U-Net: Detailed Overview

**Wave-U-Net** is a deep learning model primarily used for **audio source separation** tasks, such as separating vocals from music or isolating different instruments in a song. Unlike traditional U-Net models, which operate in the time-frequency domain, **Wave-U-Net** works directly in the time domain using raw audio waveforms, making it more efficient for tasks like music separation or noise reduction.

![image](https://github.com/user-attachments/assets/c4b56284-2462-4057-acb2-6d71b382f559)


## 1. Architecture Overview

Wave-U-Net architecture is inspired by U-Net, which was originally designed for image segmentation but adapted here for 1D audio signals. The key concept is to use **encoder-decoder networks** with **skip connections** to preserve information from input audio and achieve better reconstruction after processing.

- **Input**: Raw audio waveform, typically of a fixed length, e.g., 4 seconds at 44.1 kHz sampling rate.
- **Output**: Separated audio sources (e.g., instrumental and vocal tracks).

## 2. Components of Wave-U-Net

### a. 1D Convolutions

Wave-U-Net uses **1D convolutional layers** instead of 2D convolutions (as in image-based U-Net). This allows the model to process audio waveforms directly in the time domain.

- Each 1D convolutional layer applies a set of filters to the input, generating feature maps that capture time-domain information.
- The choice of **kernel size** and **stride** helps determine how much of the audio signal is analyzed at a time and the receptive field of the model.

### b. Downsampling (Encoding)

In the encoding path, the model progressively reduces the length of the audio features using strided convolutions (or max pooling). This is analogous to extracting low-level features from the audio, akin to capturing different frequencies and textures in the sound.

- The encoder reduces the size of the time-domain signal while increasing the number of feature maps. This creates a hierarchical representation of the input, allowing the model to focus on both high and low-level features.

### c. Upsampling (Decoding)

In the decoding path, the model gradually increases the resolution of the audio features using **transposed convolutions** (or upsampling). This reconstructs the separated sources at the original resolution.

- The decoder takes the low-resolution features from the encoder and upsamples them, producing higher-resolution outputs as it goes. The final output should ideally have the same length as the input waveform.

### d. Skip Connections

Skip connections are a crucial part of the Wave-U-Net architecture, enabling the model to pass information from earlier layers directly to later layers.

- These connections help the model recover spatial (time-domain) information lost during downsampling, allowing it to generate more accurate and detailed reconstructions of the audio sources.

### e. Final Layer

The final layer usually consists of a **linear transformation** (often implemented as a 1x1 convolution) that maps the separated features to the required output (e.g., instrumental or vocal track).

## 3. Mathematical Formulation

### a. 1D Convolution

Each convolution operation in the Wave-U-Net is defined as follows:

<img width="478" alt="image" src="https://github.com/user-attachments/assets/ab4dfc84-09cb-48ed-9906-b385f8833bef">


### b. Strided Convolutions for Downsampling

<img width="574" alt="image" src="https://github.com/user-attachments/assets/be403c25-b440-46bd-a6f6-5a9f24a6b4a3">


### c. Transposed Convolutions for Upsampling

In the upsampling path, transposed convolutions are used to increase the size of the feature maps.

<img width="491" alt="image" src="https://github.com/user-attachments/assets/e38b45cf-bd9f-4d60-9e64-992460b9c9e7">

This operation essentially "reverses" the downsampling process, generating a higher-resolution output.

## 4. Loss Function

For source separation tasks, the loss function typically used is the **Mean Squared Error (MSE)** between the predicted waveform and the ground truth waveform for each separated source.

<img width="519" alt="image" src="https://github.com/user-attachments/assets/3c6b1217-94ef-48d2-9faa-6095907027d0">


Other loss functions, such as **Signal-to-Distortion Ratio (SDR)**, can also be used depending on the specific task.

## 5. Applications

- **Music Source Separation**: Wave-U-Net is commonly used to separate different instruments (e.g., vocals, drums, bass) from mixed audio tracks.
- **Speech Enhancement**: It can be applied to denoise or separate speech from noisy environments.
- **Environmental Sound Separation**: In applications such as wildlife monitoring or urban sound analysis, Wave-U-Net helps isolate specific sounds (e.g., cars, birds, etc.).

## 6. Advantages of Wave-U-Net

- **Works in Time Domain**: Unlike many traditional approaches that rely on time-frequency representations (e.g., spectrograms), Wave-U-Net operates directly in the time domain, which can preserve more detailed temporal information.
- **Efficient Architecture**: The U-Net structure, with its use of skip connections, allows the model to reconstruct audio signals with high fidelity, even after aggressive downsampling.
- **Versatility**: It can be applied to a wide range of audio-related tasks, such as speech separation, noise reduction, and music demixing.

## 7. Challenges and Limitations

- **Computational Intensity**: Training Wave-U-Net on large datasets (especially high-fidelity audio) can be computationally expensive due to the large number of parameters and operations involved.
- **Generalization**: Although it performs well on certain tasks, the model may not generalize well to highly diverse audio domains without careful training and tuning.

## 8. Variants of Wave-U-Net

Over time, several modifications have been made to Wave-U-Net to improve its performance:

- **Multi-Scale Wave-U-Net**: Combines multiple scales of time-domain data for more robust source separation.
- **Dilated Convolutions**: Some versions of Wave-U-Net incorporate dilated convolutions to increase the receptive field without increasing the number of parameters, helping the model capture long-range dependencies in audio.

---
**In summary**, Wave-U-Net is a powerful tool for audio source separation that leverages the strengths of U-Net architecture but adapts it for 1D audio signals in the time domain. Its combination of downsampling, upsampling, and skip connections allows it to process and separate audio with high fidelity, making it an essential tool for modern audio engineering tasks.


### MUSDB18HQ Dataset

#### Full Form
**MUSDB18HQ** stands for **Music Source Separation Database 2018 High Quality**. It is a dataset designed for evaluating and training algorithms in the domain of music source separation.

#### Details of MUSDB18HQ Dataset

- **Purpose**: MUSDB18HQ is specifically designed for **music source separation** tasks, such as isolating vocals, drums, bass, and other instrumental tracks from mixed music recordings. It provides high-quality, well-organized data for training and evaluating models that aim to separate individual audio sources from a mixture.

- **Content**: The dataset consists of 150 tracks of high-quality music, which are divided into 50 training tracks, 25 validation tracks, and 75 test tracks. Each track is available in both stereo and separated mono channels.

- **Format**:
  - **Audio Files**: The dataset includes both the mixed audio files and the separated sources. Each track is provided as WAV files with a 44.1 kHz sampling rate, which ensures high audio fidelity.
  - **Source Tracks**: The sources are separated into different stems, such as vocals, drums, bass, and other instruments.
  
- **Quality**: The "HQ" in MUSDB18HQ indicates that the dataset features high-quality audio recordings. The tracks are recorded and processed to ensure minimal noise and artifacts, providing a clean and accurate representation of each source.

- **Usage in Wave-U-Net Implementation**:
  - **Training**: MUSDB18HQ is used to train Wave-U-Net models because it provides diverse and high-quality examples of music tracks with well-separated sources. This allows the model to learn how to accurately separate different audio sources from mixed recordings.
  - **Evaluation**: The dataset is also used for evaluating the performance of source separation algorithms. By using a standardized set of tracks with known source components, researchers can measure how well their models perform in isolating the different sources.
  - **Benchmarking**: MUSDB18HQ serves as a benchmark dataset in the field of music source separation, enabling comparisons between different algorithms and approaches. Its high-quality recordings provide a consistent basis for evaluating and improving source separation techniques.

- **Key Features**:
  - **High-Quality Audio**: Ensures accurate training and evaluation with minimal distortion.
  - **Diverse Content**: Includes a variety of music genres and sources, making it suitable for generalizing across different types of music.
  - **Standardized Format**: Provides data in a consistent format, making it easier to integrate into various models and frameworks.

The MUSDB18HQ dataset is widely recognized in the field of music information retrieval and audio processing, and it plays a crucial role in developing and testing advanced source separation models like Wave-U-Net.

