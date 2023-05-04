<br><br><br>
# henesys2ellinia

This is just for educational purpose, I didn't use it in any malicious or commercial way

- The copyright of the training images belongs to [Nexon](https://www.nexon.com/Home/Game) game - [Maplestory](https://maplestory.nexon.com/Home/Main)
- So I can't share the training images I've used, please understand

For those who like Maplestory or those who have played it, I hope this would be a little bit of fun for them.

<br>

- (23/04/24) elnath2aquarium update
- (23/04/29) perion2twilight_perion update
- (23/05/05) chewchew_island2lachelein update

<b>Note</b> : Current version of [my implementation](CycleGAN.ipynb) uses separate data augmentation layer. One might slightly reduce the training time by letting the tf.data.dataset object to do that. Currently all the data preprocessing steps(except augmentation) make use of it to  parallelize the I/O & GPU works. 

<br>
<br>


# About this project

Henesys and ellinia are two of the most popular cities in maple world. And both cities have their own styles.

Think about particular henesys map. What would this map look like if it were in ellinia? (or in the opposite direction)

My goal is image-to-image translation between cities in maple world.

But here's the problem, there's no (henesys_city, ellinia_city) pair which have same structure. (i.e we don't have paired dataset)

[CycleGAN](https://junyanz.github.io/CycleGAN/) will be used to deal with this unpaired image-to-image translation task.

You could see the details at the following links

| [CycleGAN paper](https://arxiv.org/abs/1703.10593)
| [Official homepage](https://junyanz.github.io/CycleGAN/) | 

<br>

- First, some result images & videos are provided
- And then, training details

<br>
<br>

# Some results

Recommend you to check the images & videos at result images & result videos directory

Since the scale(in pixels) in the original game is preserved, feel free to open the files & zoom in to see the details.

## henesys2ellinia

- ellinia2henesys training performance
![e2h training result](result_images/henesys2ellinia/training_performance/ellinia2henesys%20result5.jpeg)

- ellinia2henesys test performance
![e2h test result](result_images/henesys2ellinia/test_performance/ellinia2henesys%20result1.jpeg)

- henesys2ellinia training performance
![h2e training result](result_images/henesys2ellinia/training_performance/henesys2ellinia%20result7.jpeg)

- henesys2ellinia test performance
![h2e test result](result_images/henesys2ellinia/test_performance/henesys2ellinia%20result6.jpeg)

- [more training results](https://github.com/chk7082/henesys2ellinia/tree/master/result_images/henesys2ellinia/training_performance)
- [more test results](https://github.com/chk7082/henesys2ellinia/tree/master/result_images/henesys2ellinia/test_performance)

- video results
  ![where the forest sings.gif](result_videos/%5Be2h%5D%20where%20the%20forest%20sings.gif)
  ![henesys north hill.gif](result_videos/%5Bh2e%5D%20henesys%20north%20hill.gif)

<br>

- From now on, only the training performance will be considered.

## elnath2aquarium

- elnath2aquarium performance
![e2a training result](result_images/elnath2aquarium/elnath2aquarium%20result9.jpeg)

- aquarium2elnath performance
![a2e training result](result_images/elnath2aquarium/aquarium2elnath%20result3.jpeg)

- [more image results](https://github.com/chk7082/henesys2ellinia/tree/master/result_images/elnath2aquarium/)

- video results
  ![storm wing.gif](result_videos/%5Be2a%5D%20storm%20wing.gif)
  ![red coral forest.gif](result_videos/%5Ba2e%5D%20red%20coral%20forest.gif)

<br>

## perion2twilight_perion

- perion2twilight_perion performance
![p2t training result](result_images/perion2twilight_perion/perion2twilight_perion%20result7.jpeg)

- twilight_perion2perion performance
![t2p training result](result_images/perion2twilight_perion/twilight_perion2perion%20result9.jpeg)

- [more image results](https://github.com/chk7082/henesys2ellinia/tree/master/result_images/perion2twilight_perion/)

- video results
  ![perion.gif](result_videos/%5Bp2t%5D%20perion.gif)
  ![abandoned excavation site 2](result_videos/%5Bt2p%5D%20abandoned%20excavation%20site%202.gif)

<br>

## chewchew_island2lachelein

- chewchew_island2lachelein performance
![c2l training result](result_images/chewchew_island2lachelein/chewchew_island2lachelein%20result10.jpeg)

- lachelein2chewchew_island performance
![l2c training result](result_images/chewchew_island2lachelein/lachelein2chewchew_island%20result7.jpeg)

- [more image results](https://github.com/chk7082/henesys2ellinia/tree/master/result_images/chewchew_island2lachelein/)

- video results
  ![chewchew island.gif](result_videos/%5Bc2l%5D%20chewchew%20island.gif)
  ![revelation place 1.gif](result_videos/%5Bl2c%5D%20revelation%20place%201.gif)


<br><br>

# Data Preparation

[WzComparerR2](https://github.com/KENNYSOFT/WzComparerR2/releases) was used to extract images, by following the [instruction](https://m.blog.naver.com/yeji__tok/221703890764).

The npc & monster & portal are excluded.

<b>Note</b> : Map can be decomposed into two parts, foreground & background. When extracting images, foreground part is invariant to the position you're looking at, but background part isn't.

- Again, Background part might be composed of multiple layers. Where each layers have different moving ratio w.r.t the position we're looking at.

By using this property, one could extract some large maps multiple times at different positions. By doing this, we could collect sufficient number of images for each distribution. And extra maps were also considered if those resembles the distribution of our targets. (such as, maple island ~ henesys, some event maps)

All the images are acquired only from [WzComparerR2](https://github.com/KENNYSOFT/WzComparerR2/releases), just to maintain the original & consistent scale of pixels. 

<br><br>

# CycleGAN

CycleGAN is a generative adversial network for unpaired image-to-image translation task. It's composed of 2 generators & 2 discriminators. Where generator's job is to map one distribution image to another, and discriminator's job is to discriminate whether the given image is real or fake.

<br>

## Loss

Adversial loss makes the result realistic(indistinguishable from real distribution). Among those, [least square loss from LSGAN](https://arxiv.org/abs/1611.04076) was used as author did.

Cycle consistency loss was introduced in this paper. Basically it means, if we transfrom the image from A to another distribution by using one generator, and then transform it back by using the other generator(which forms a cycle), we want those two to be equal. So cycle consistency loss is defined as the L1 distance between those two images in pixel space.

The optional identity loss (described in paper) was omitted.

- generator_loss = lsgan_adversial_loss + lambda * cycle_consistency_loss

  - lambda = 10 was used.

- discriminator_loss = lsgan_adversial_loss / 2

  - discriminator objective was divided by 2, just to slow down the rate at which it learns.

<br>

## Generator

Each generator is composed of 2 contracting blocks & 9 residual blocks & 2 expanding blocks.
- c7s1-64, d128, d256, R256 * 9, u128, u64, c7s1-3 (following the notation of paper in Section 7.2)

<br>

## Discriminator

Each discriminator is [PatchGAN](https://arxiv.org/abs/1611.07004v3) discriminator(introduced in [pix2pix](https://arxiv.org/abs/1611.07004v3)) of receptive field : 70 (in pixels)
- C64 - C128 - C256 - C512 - output (also following the notation of paper in Section 7.2)

<br>

For more details, refer | [CycleGAN paper](https://arxiv.org/abs/1703.10593) | [official homepage](https://junyanz.github.io/CycleGAN/) | [my implementation](CycleGAN.ipynb) |

<br><br>

# Training details

All the model was trained from scratch with Adam optimizer (learning_rate=0.0002, beta_1=0.5, beta_2=0.999). All weights are initialized from a gaussian distribution with mean=0 and standard deviation=0.02.

Since we used the generator with 4x downsampling & 4x upsampling, in order to compute cycle consistency loss, height and width should be multiple of 4(otherwise, we need extra modification).

The network is fully convolutional. We don't use any fully connected layer. Since convolution can be applied to arbitrary size image, we trained the network with randomly cropped image of size (height, width) = (480, 480) (which was horizontally flipped with 50% chance), and at inference time, we applied it to the original images.

Video could be interpreted as the sequence of images(frames). So we mimicked [horse2zebra video](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/blob/master/imgs/horse2zebra.gif) by applying the generator for frames. 
- recording of the original video was done in [WzComparerR2](https://github.com/KENNYSOFT/WzComparerR2/releases) by [windows xbox game bar](https://support.microsoft.com/ko-kr/windows/xbox-game-bar%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-pc%EC%97%90%EC%84%9C-%EA%B2%8C%EC%9E%84-%ED%81%B4%EB%A6%BD-%EB%85%B9%ED%99%94-2f477001-54d4-1276-9144-b0416a307f3c).
- to reduce memory we choose low fps

<b>Note</b> : We didn't impose explicit temporal consistency to the model, but the result is quite good (at least I think). It seems that the combination of continuous change of background & foreground and cycle consistency loss makes it continuous.

<br>

## henesys2ellinia

- training dataset : 173 images from henesys & 135 images from ellinia (Batch_size = 1, 1 epoch = 135 update)
- trained number of epoch : 2000
  - after 1000 epoch, we linearly decayed the learning rate to 0
- training time : roughly 100 hours with NVIDIA Tesla T4

<br>

## elnath2aquarium

- training dataset : 159 images from elnath & 147 images from aquarium (Batch_size = 1, 1 epoch = 147 update)
- trained number of epoch : 2000
  - after 1000 epoch, we linearly decayed the learning rate to 0
- training time : roughly 100 hours with NVIDIA Tesla T4

<br>

## perion2twilight_perion

- training dataset : 189 images from perion & 220 images from twilight_perion (Batch_size = 1, 1 epoch = 189 update)
- trained number of epoch : 1200
  - after 600 epoch, we linearly decayed the learning rate to 0
- training time : roughly 80 hours with NVIDIA Tesla T4

<br>

## chewchew_island2lachelein

- training dataset : 250 images from chewchew_island & 156 images from lachelein (Batch_size = 1, 1 epoch = 156 update)
- trained number of epoch : 2000
  - after 1000 epoch, we linearly decayed the learning rate to 0
- training time : roughly 100 hours with NVIDIA Tesla T4


<br><br>

