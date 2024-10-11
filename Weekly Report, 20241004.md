Weekly Report (20241004)



This week I continued some of my coding tasks, mainly discussing my results with Hanjing to confirm the reasonability and asking for suggestions. Before, I have been strictly following the hyperparameter settings written in the past papers, but some papers might do not completely write their training details, so my network training may not reach a good state. Hanjing suggested that I should take the actual performance of the model first, and can directly use the well-trained model from the Internet. And I think later I may should not force me to align the settings in the paper during my own training.

 

Because I have doing experiments for 2 weeks, so the main focus for this week was reading papers, especially the topics of Bayesian Deep Learning X Generative Models (Transformers/Diffusion Models).

 

1. Firstly, I read Hanjing and Hongji's paper about Probabilistic Transformers (https://openaccess.thecvf.com/content/CVPR2022/papers/Guo_Uncertainty-Guided_Probabilistic_Transformer_for_Complex_Action_Recognition_CVPR_2022_paper.pdf), which uses random variables to encoding attention scores so that we can measure the uncertainty of the output(downstream classification tasks);
2. I also read the paper "Hyper-Diffusion: Estimating Epistemic and Aleatoric Uncertainty with a Single Model" (https://arxiv.org/pdf/2402.03478v1), which employs a hyper-network to generate the paremeters of Diffusion Models ( learn the parameters distributions). Then estimate the uncertainty of diffusion model with MC sampling (getting parameters from the hyper-network, is like, sampling from the posterior p(theta|D)).
3. I read the paper "BayesDiff: Estimating Pixel-wise Uncertainty in Diffusion via Bayesian Inference" (https://arxiv.org/pdf/2310.11142), which employs a Last-Layer-Laplace-Approximation in the U-Net structure of Diffusion Model. This allows the model to learn, during each noise-removing process, not only the value(mean) of the removed noise, but also the (estimated) variance of the removed noise. Moreover, this distribution of removed noise lets us have a distribution of the recovered image (A_t) every time, and in the final result, we will have the mean of A_0 (output image), the variance of A_0 ( a kind of uncertainty , pixel-wisely). 

 

Beyond that, I came up with a naive thought about using aleatoric uncertainty to guide a image denoising task. And to validate this assumption, I started some experiments (based on CIFAR-10 and Resnet18; DnCNN is used to denoise; Add random noise to CIFAR-10 data to get paired noise-clean dataset ).

 

- I want to confirm that for the classification tasks with Resnet18 (trained with clean data), clean data leads lower aleatoric uncertainty than noisy data.
- I want to confirm that denoised data will have lower aleatoric uncertainty than noisy data, and if our denoiser is well-trained, the aleatoric uncertainty of denoised data will have a same level with pure clean data.
- Then I want to introduce the aleatoric uncertainty to the training or evaluation of denoiser, such as, by designing loss functions. (epistemic uncertainty may also need to be considered).