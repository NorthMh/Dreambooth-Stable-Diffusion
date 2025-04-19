# Dreambooth on Stable Diffusion

**This is an implementtaion of Dreanbooth-Stable-Diffusion based on the work of XavierXiao**.

See original work: https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

**I created this fork only for the course project, without any illegal purpose**.  Here I will give some further suggestions based on XavierXiao's guidance document and point out possible problems in the project.
In addition, I list some results with a single academic building image in the generate folder. You are welcome to have a look.

## Usage

**waiting for done, preparing work...**

## Some potential deployment issues
**The layout is not finished yet, I just list the problems and solutions**

### No mid ckpt
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

### GPU mem not enough
- You may find " trainer_config["accelerator"] = "ddp" " in main.py
This is a parallel training strategy in PyTorch that automatically splits the training dataset into multiple GPUs or multiple machines. Setting it up in a single GPU environment may increase unnecessary inter-process communication overhead, resulting in a slight decrease in training speed (5-10%), just comment it out.

- Adjusting the size of the generated images and reducing the number of images generated at a time are good ways to reduce GPU memory.




