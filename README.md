## Cross-dataset Learning for Generalizable Land Use Classification (PyTorch)

Training and testing utilities for fine-tuning remote sensing feature extractors on multiple datasets.

---

### Dependencies:

You can use the provided .yml Conda environment file to easily install dependencies into a separate environment:
```
# From repo root
cd env
conda env create -f rsfinetuning.yml
conda activate rsfinetuning
```

---
 
### Setup :

Our code is inspired from [[Cycle-GAN](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)].

The following datasets can be used for training and testing:
* AID
* Brazilian Coffe Scenes
* PatternNet
* RESISC45
* RSICB-256
* RSSCN7
* SIRI-WHU
* UCM
* WHU-RS19

Dataroots must be hardcoded in the corresponding data/$DATASETNAME_dataset.py file.
A script will automatically generate a dataset file in the dataroot folder the first time a dataset is used to avoid systematic parsing. 

---

### Training :

Run the following script to train (replace $CHECKPOINTS_DIR with the location where you want to save networks and tensorboard data):

<details>
<summary><b>Training on multiple datasets</b></summary><br/>

```
python3 train.py --batch-size 32 --batch-test 512 --checkpoints-dir $CHECKPOINTS_DIR --dim 512 --gpu-ids 0 --imsize 256 --lr 0.0001 --model deepdesc --net resnet50 --niter 10 --niter-decay 5 --save-epoch-freq 1 --val-freq 1 --test-dataset aid --whiten
```

</details>

---

###  Testing :

Test performance can be evaluated while training using the --test-datasets option (multiple datasets possible), or separately using a trained model loaded from a run folder $RUN:

<details>
<summary><b>Testing</b></summary><br/>

```
python3 test.py --model deepdesc --load-from $RUN --net resnet50 --dim 512 --whiten --gpu-ids 0 --test-datasets aid --batch-test 512
```
</details>

---

### Acknowledgments :

Thanks to :
* https://github.com/filipradenovic/cnnimageretrieval-pytorch for the GeM descriptor
* https://github.com/peymanbateni/simple-cnaps for the computation of Mahalanobis distance
* https://github.com/Andrew-Brown1/Smooth_AP for the Smooth AP loss
* https://junyanz.github.io/CycleGAN/ for the code structure
