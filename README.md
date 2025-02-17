# DeepFakeMnist+

This page contain the download link for our proposed data [DeepFake Mnist+](https://arxiv.org/abs/2108.07949), [download link](https://1fichier.com/?do5lezggwcnpg49m28wh).

The paper has been accepted by ICCV21 workshop: AIM2021 (Advances in Image Manipulation workshop).

We will update code and trained models for re-production later.

# Dataset Introdution

It contains video animation data generated by [First Order Motion Model for Image Animation](https://papers.nips.cc/paper/2019/file/31c0b36aef265d9221af80872ceb62f9-Paper.pdf)

The dataset contain 10,000 real videos selected from [VoxCeleb1](https://www.robots.ox.ac.uk/~vgg/data/voxceleb/vox1.html) and 10,000 animation videos generated by using these 10,000 real videos as sources.

![alt text](https://github.com/huangjiadidi/DeepFakeMnist/blob/main/readme_src/action_clip.png)

Image animation means given a victim's image, and a driving video (which provide action), the animation result is a video that the victim perform same action just like the driving video did. 

The animation videos is animated with 10 different actions, including: Blink, Open mouth, Yaw, Nod, Right slope head, Left slope head, Look up and smile, surprise and embarrassment. The driving videos with different actions are collected by multiple actors. In addition, we select the embarrassment videos from [ADFES](https://aice.uva.nl/research-tools/adfes-stimulus-set/adfes-stimulus-set.html?cb) as the driving videos for embarrassment. We generated 1000 videos for each action.

In order to select high-quality animation video, all generated videos have passed and able to proof the recent liveness detectors in the markets. We select the two detectors from [Baidu](https://ai.baidu.com/tech/face/faceliveness) and [Tianyan](https://www.tianyandata.cn/) to help us to filter out the high-quailty results.

# Video Generation Pipeline
![alt text](https://github.com/huangjiadidi/DeepFakeMnist/blob/main/readme_src/dataset_pipeline.png)

# Animation Video Detection Pipeline
![alt text](https://github.com/huangjiadidi/DeepFakeMnist/blob/main/readme_src/dataset_detection_pipeline.png)


# Dataset Evaluation - action impact

**Accuracy under different network and compression level**


|                     | Resnet50  | Resnet101 | Resnet152 | XceptionNet | MesoNet |
| :-----------------: |:---------:| :--------:| :--------:| :----------:| :------:|
| raw                 | 96.58%    | 96.64%    | 96.18%    | 92.38%      | 60.39%  |
| light compression   | 94.32%    | 94.87%    | 94.90%    | 85.52%      | 58.58%  |
| heavy compression   | 91.49%    | 90.44%    | 91.27%    | 83.14%      | 57.90%  |

notes: **raw** means original videos, light compression means the videos are compressed with H.264 in c23 compression ratio, heavy compression means the videos are compressed with H.264 in c40 compression ratio.

**Accuracy on detecting animation videos under different actions**

*1. The detection accuracy for the Resnet50 modelstrained with one specific actions videos.*

![alt text](https://github.com/huangjiadidi/DeepFakeMnist/blob/main/readme_src/each_action_train_only.png)

The result indicates that using large movement action videos to train classifiers couldlead to better performance for detecting new actions’ videos.The actions that only include small changes, e.g., smilingand blinking, cannot provide sufficient information for thenetworks to adapt to unseen videos.

*2. The detection accuracy for each action under differ-ent video quality.*

![alt text](https://github.com/huangjiadidi/DeepFakeMnist/blob/main/readme_src/each_action_merge.png)

We observe that some hard-to-detect actions, e.g., right and left slope, could providemore generalization if we only use those actions’ videos fortraining. On the contrary, the models trained with the videosof easy-to-detect actions, e.g., smiling and blinking, showingpoorer performances for adapting unseen actions.

# Dataset Evaluation - video quailty impact
We notice that the video quality could significantly affect the detection performance. The results below are the models trained with one video quailty and tested in other video quality. It indicates that training with single video quality cannot handle the data under other video quailty.

|          | Raw->LC   | Raw->HC | LC->Raw | LC->HC | HC->Raw | HC->LC |
| :------: |:---------:| :------:| :------:| :-----:| :------:| :-----:|
| Resnet50 | 85.52%    | 71.3%   | 95.16%  | 82.98% | 59%     | 61.02% |
| Resnet152| 79.23%    | 68.12%  | 82.33%  | 73.60% | 76.72%  | 76.69% |


We explored a new training strategy that combine videos in different compression ratio to train our final model.

|                     | Resnet50  | Resnet152 | 
| :-----------------: |:---------:| :--------:| 
| raw                 | 93.57%    | 95.78%    | 
| light compression   | 90.69%    | 92.11%    | 
| heavy compression   | 85.56%    | 88.32%    |

The results show that the mixed quailty training could allow a single model to handle videos under different compression ratio.


# Citation

@misc{huang2021deepfake,
      title={DeepFake MNIST+: A DeepFake Facial Animation Dataset}, 
      author={Jiajun Huang and Xueyu Wang and Bo Du and Pei Du and Chang Xu},
      year={2021},
      eprint={2108.07949},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}



