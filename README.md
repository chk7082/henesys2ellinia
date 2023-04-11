<br><br><br>
# henesys2ellinia

This is just for educational purpose, I didn't use it in any malicious or commercial way

- The copyright of the training images belongs to [Nexon](https://www.nexon.com/Home/Game) game - [Maplestory](https://maplestory.nexon.com/Home/Main)
- So I can't share the training images I've used, please understand

<br>
<br>


# About this project

Henesys and ellinia are two of the most popular cities in maple world. And both cities have their own styles.

Think about particular henesys map. What would this map look like if it were in ellinia? (or in the opposite direction)

Our goal is image-to-image translation between cities in maple world.

But here's the problem, there's no (henesys_city, ellinia_city) pair which have same structure. (i.e we don't have paired dataset)

We will use [CycleGAN](https://junyanz.github.io/CycleGAN/) to deal with this unpaired image-to-image translation task.

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

Since we've preserved the scale(in pixels) in the original game, feel free to zoom in to see the details.

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
  - [where the forest sings.mp4](https://github.com/chk7082/henesys2ellinia/blob/master/result_videos/%5Be2h%5D%20where%20the%20forest%20sings.mp4)
  - [henesys north hill.mp4](https://github.com/chk7082/henesys2ellinia/blob/master/result_videos/%5Bh2e%5D%20henesys%20north%20hill.mp4)

<br>

## Will be added soon

<br><br>

# Data Preparation

<br><br><br><br>
---
original scale

code link

cyclegan explanation