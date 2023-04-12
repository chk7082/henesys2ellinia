<br><br><br>
# henesys2ellinia

This is just for educational purpose, I didn't use it in any malicious or commercial way

- The copyright of the training images belongs to [Nexon](https://www.nexon.com/Home/Game) game - [Maplestory](https://maplestory.nexon.com/Home/Main)
- So I can't share the training images I've used, please understand

For those who like maple or those who have played it, I hope this would be a little bit of fun for them.

<br>
<br>


# About this project

Henesys and ellinia are two of the most popular cities in maple world. And both cities have their own styles.

Think about particular henesys map. What would this map look like if it were in ellinia? (or in the opposite direction)

My goal is image-to-image translation between cities in maple world.

But here's the problem, there's no (henesys_city, ellinia_city) pair which have same structure. (i.e we don't have paired dataset)

I will use [CycleGAN](https://junyanz.github.io/CycleGAN/) to deal with this unpaired image-to-image translation task.

You could see the details at the following links

| [CycleGAN paper](https://arxiv.org/abs/1703.10593)
| [Official homepage](https://junyanz.github.io/CycleGAN/) | 

<br>

- First, you could see some result images & videos 
- And then, training details are provided

<br>
<br>

# Some results

Recommend you to check the images & videos at result images & result videos directory

Since I've preserved the scale(in pixels) in the original game, feel free to go to the result_images & result_videos folder & open the files & zoom in to see the details.

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

## Will be added soon

<br><br>

# Data Preparation

I've used [WzComparerR2](https://github.com/KENNYSOFT/WzComparerR2/releases) to extract images, by following the [instruction](https://m.blog.naver.com/yeji__tok/221703890764).

I turned off the npc & monster & portal.

<b>Note</b> : Map can be decomposed into two parts, foreground & background. When extracting images, foreground part is invariant to the position you're looking at, but background part isn't.

By using this property, I extracted some large maps multiple times at different positions. Because I wanted to collect sufficient number of images for each distribution. And I've included some extra maps if those resembles the distribution of our targets. (such as, maple island ~ henesys, some event maps)

All the images are acquired only from [WzComparerR2](https://github.com/KENNYSOFT/WzComparerR2/releases), since I wanted to maintain the original & consistent scale of pixels. 

<br><br>

# CycleGAN

CycleGAN is a generative adversial network for unpaired image-to-image translation task. It's composed of 2 generators & 2 discriminators. Where generator's job is to map one distribution image to another, and discriminator's job is to discriminate whether the given image is real or fake.

<br>

## Loss

Adversial loss makes the result realistic(indistinguishable from real distribution). I used [least square loss from LSGAN](https://arxiv.org/abs/1611.04076) for adversial loss as author did.

Cycle consistency loss was introduced in this paper. Basically it means, if we transfrom the image from A to another distribution by using one generator, and then transform it back by using the other generator(which forms a cycle), we want those two to be equal. So cycle consistency loss is defined as the L1 distance between those two images.

I omitted the optional identity loss described in paper

<br>

## Generator

I used the generator with 2 contracting blocks & 9 residual blocks & 2 expanding blocks.
- c7s1-64, d128, d256, R256 * 9, u128, u64, c7s1-3 (following the notation of paper in Section 7.2)

<br>

## Discriminator

I used the [PatchGAN](https://arxiv.org/abs/1611.07004v3) discriminator(introduced in [pix2pix](https://arxiv.org/abs/1611.07004v3)) of receptive field : 70 (in pixels)


<br>

For more details, refer | [CycleGAN paper](https://arxiv.org/abs/1703.10593) | [official homepage](https://junyanz.github.io/CycleGAN/) | [my implementation](CycleGAN.ipynb) |

<br><br>

# Training details


training time

learning rate decay

gpu

since it's fullly convolutional, we could apply it to arbitrary size images (even the original one)