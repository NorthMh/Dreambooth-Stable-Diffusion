# Dreambooth on Stable Diffusion

**This is an implementtaion of Dreanbooth-Stable-Diffusion based on the work of XavierXiao**.

See original work: https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

**I created this fork only for the course project, without any illegal purpose**.  Here I will give some further suggestions based on XavierXiao's guidance document and point out possible problems in the project.
In addition, I will show some results as well. 

You are welcome to refer to it.

## Usage

waiting for done, preparing work...



```
 target: main.DataModuleFromConfig
  params:
    batch_size: 1
    num_workers: 2
    wrap: false
    train:
      target: ldm.data.personalized.PersonalizedBase
      params:
        size: 512
        set: train
        per_image_tokens: false
        repeats: 100 

```

This parameter may cause errors in saving intermediate ckpts based on the number of images you input.

Assume every_n_train_steps: 500

- When you pass in 1 image and repeat = 100
  - That is, 1 epoch will perform 1+100 steps
- If you input 5 images and repeat = 100
  - That is, 1 epoch will perform 5+500, so your first epoch will execute 505 steps

However, for the evaluation indicator: **Monitoring val/loss_simple_ema as checkpoint metric**.

Therefore, the ckpt must be saved before the first epoch is completed, but the val stage has not yet started, and the system will report the error of **"Cannot find val/loss_simple_ema"** indicator.

In summary, it is recommended to dynamically adjust the parameter repeat according to the input image. Here I use about 100 steps per epoch for training.
