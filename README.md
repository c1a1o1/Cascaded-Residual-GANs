# Cascaded-Residual-GANs

## Network Architecture
The proposed framework is shown in the figure. It has two reciprocal directions of translation, i.e. SAR to Optical and Optical to SAR. Each direction consists of two adversarial deep networks, i.e. a multi-scale convolutional encoder-and-decoder network as the translator (generator) vs. a convolutional network as the discriminator. The translator takes in SAR image, maps it to the latent space via the encoder, and then remaps it to a translated optical image. The discriminator takes in both the translated optical image and the true optical image which is co-registered with the original SAR image, and outputs the classification results. The discriminator learns to identify the translated optical images from the true optical images, while the translator network learns to convert the SAR image to an optical image as realistic as possible to fool the discriminator. On the other direction, the network is constructed exactly in the same manner with the only difference being optical as input and SAR as translated image.
![Supervised learning diagram](https://github.com/Shilling818/Cascaded-Residual-GANs/blob/master/image_fold/Supervised%20learning.png)

The below figure shows the architecture and parameters of the translator network. It follows the main structure of the U-net and the Pix2Pix with certain modifications. On the encoder side, the input image is convolved at one scale and downsampled to the next scale repeatedly for 6 times. On the decoder side, the latent feature map is deconvolved and upsampled back to the original scales. Notably, we include direct links from encoder to decoder. In addition, the input image is downsampled accordingly to different scales and a residual link is added from input to decoder, which means the network learns the difference between input and output. For the residual links, the downsampled inputs are attached (but not added) to the feature maps in the decoders. 
![The translator of GANs](https://github.com/Shilling818/Cascaded-Residual-GANs/blob/master/image_fold/GAN-translator.png)

This paper also explores the possibility of unsupervised learning with unpaired SAR and optical images. CycleGAN proposes a cyclic loop which could be leveraged for this purpose. The SAR image is first fed to the translator A and synthesizes fake optical image. Then the fake optical image is used to synthesize the cyclic fake SAR images by the translator B. On the other hand, the optical image is used to synthesize fake SAR image which is then further used to synthesize the cyclic fake optical images. The cyclic images are compared with the corresponding true images in a pixel-by-pixel fashion, while the synthesized fake images are fed into the ‘critic’ discriminator networks.
![Unsupervised learning diagram](https://github.com/Shilling818/Cascaded-Residual-GANs/blob/master/image_fold/Unsupervised%20learning.png)

Two pairs of translated results are shown in the figure.
![Translated results](https://github.com/Shilling818/Cascaded-Residual-GANs/blob/master/image_fold/results.png)


## Programs
The programs in the root directory:
- loss.py : the definition of some loss functions
- model.py : the definition of some model functions
- network.py : the network architecture of our proposed cascaded residual GAN


./Comparsion with existing translation networks (some mainstream networks for comparison)
- network_CycleGAN_Comp.py : the CycleGAN architecture
- network_U-Net_Comp.py : the U-Net architecture
- network_WGAN_Comp.py : the WGAN architecture


./Enhancement with unsupervised Learning (developed for unsupervised learning and indeed improve the performance of translation results)


./Evaluation (some metrics for evaluting the translation results)
- fid.py : Fréchet inception distance between two datasets
- l1.m : the L1-distance between two images
- PSNR.m : the Peak Signal-to-Noise Ratio between two images
- SSIM.m : the Structural Similarity between two images

## Misc Notes
If you use this code, please cite
```text
@article{fu2019reciprocal,
  title={Reciprocal Translation between SAR and Optical Remote Sensing Images with Cascaded-Residual Adversarial Networks},
  author={Fu, Shilei and Xu, Feng and Jin, Ya-Qiu},
  journal={arXiv preprint arXiv:1901.08236},
  year={2019}
}
```
