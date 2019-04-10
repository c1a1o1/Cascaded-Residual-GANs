# Cascaded-Residual-GANs

Fig. 3 shows the architecture and parameters of the translator network. It follows the main structure of the U-net and the Pix2Pix with certain modifications. On the encoder side, the input image is convolved at one scale and downsampled to the next scale repeatedly for 6 times. On the decoder side, the latent feature map is deconvolved and upsampled back to the original scales. Notably, we include direct links from encoder to decoder. In addition, the input image is downsampled accordingly to different scales and a residual link is added from input to decoder, which means the network learns the difference between input and output. For the residual links, the downsampled inputs are attached (but not added) to the feature maps in the decoders. It should be noted that similar measures were also tried e.g. a skip connection between input and output in and e.g. latent connections to every intermediate layer of the encoder in Zhu et al Thus, we believe that such multi-scale cascaded residual connections are effective in generating vivid high resolution images. In order to increase the depth of the network, at each time upsampling the feature maps, it first concatenates the encoder’s feature maps to the current ones and deconvolves. Then concatenate the residual block to the former output feature maps and deconvolve again. This results in the increase of the decoder’s receptive field. Thus the receptive field of the encoder and that of the decoder will be asymmetrical, which may degrade performance. The solution is to convolve feature maps of each scale twice in the encoder.

loss.py : the definition of some loss functions
model.py : the definition of some model functions
network.py : the network architecture of our proposed cascaded residual GAN

./Comparsion with existing translation networks (some mainstream networks for comparison)
 network_CycleGAN_Comp.py : the CycleGAN architecture
 network_U-Net_Comp.py : the U-Net architecture
 network_WGAN_Comp.py : the WGAN architecture

./Enhancement with unsupervised Learning (developed for unsupervised learning and indeed improve the performance of translation results)

./Evaluation (some metrics for evaluting the translation results)
 fid.py : Fréchet inception distance between two datasets
 l1.m : the L1-distance between two images
 PSNR.m : the Peak Signal-to-Noise Ratio between two images
 SSIM.m : the Structural Similarity between two images

If you use this code, please cite
''' text
@article{fu2019reciprocal,
  title={Reciprocal Translation between SAR and Optical Remote Sensing Images with Cascaded-Residual Adversarial Networks},
  author={Fu, Shilei and Xu, Feng and Jin, Ya-Qiu},
  journal={arXiv preprint arXiv:1901.08236},
  year={2019}
}
'''
