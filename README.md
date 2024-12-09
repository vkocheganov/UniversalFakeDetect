# Get sources
- git clone -b high-sky-release git@github.com:vkocheganov/UniversalFakeDetect.git UniversalFakeDetect_test
- cd UniversalFakeDetect_test

# Create venv:

- conda create -n fakedet_test python=3.12  -y
- conda activate fakedet_test
- pip3 install -r requirements.txt
- conda install -n fakedet_test ipykernel --update-deps --force-reinstall

# [Download](https://drive.google.com/drive/folders/1AApe9dSWeAUx_mWgw2iOEDMgfmtnqdXg?usp=sharing) best model .pth file and put it like this:
- UniversalFakeDetect_test/best

# Put dataset from test assignment to the folder of your choice (e.g. in directory UniversalFakeDetect_test/../FAKE_IMAGES/dollar_datasets_train/train/{0_real,1_fake})
# Open UniversalFakeDetect_test/unifd.ipynb jupyter notebook and redefine variables on the first lines:
- base_fold = f"/home/vkocheganov/work/research_projects/dollar/UniversalFakeDetect_test"
- result_fold = os.path.join(base_fold, 'result')
- ckpt_path = os.path.join(base_fold, "best_0") # or best_2. The best one is best_2 from last experiment
- high_sky_dataset_path = "/mnt/ssd4tb/vk/dollar/dollar_dataset"

# Check output metrics in 'result_fold' folder or via output tupled variables 'ap, r_acc0, f_acc0, acc0, r_acc1, f_acc1, acc1, best_thres'



# In order to train
- remind the folder where high-sky test-set has been downloaded (UniversalFakeDetect_test/../FAKE_IMAGES/dollar_datasets_train/train/{0_real,1_fake})
- cd UniversalFakeDetect_test
- python train.py --name=clip_vitl14 --wang2020_data_path=$PWD/../FAKE_IMAGES/dollar_datasets_train --data_mode=wang2020_vic  --arch=CLIP:ViT-L/14  --fix_backbone


There are totally 3 experiments have been hold. Tensorboard events files are stored in corresponding folders under UniversalFakeDetect_test/checkpoints. Experiments names: clip_vitl14, clip_vitl14_1, clip_vitl14_2.

Experiments clip_vitl14 and clip_vitl14_1 have identical parameters, though resulting precisions are different. Since dataset size is very small, I decided to reduce batch size from 128 (==100) to 16 and conduct clip_vitl14_2 experiment.



P.S.
1. I've started deeging into the topic by finding SoTA papers, repos. [This](https://disk.yandex.ru/i/7D8h-a3l_NZDHQ) is the file with some links I found. 

2. I also tried '[classic](https://github.com/peterwang512/CNNDetection)' method in facedetection field. Though it gave poor results. One can find my jupyter in this repo in fake.ipynb

3. Also I'm really sorry for the mess, but I did not have enough time to align with all your requirements and make all the experiments, which are logically consistent with the data. I've got vest available SoTA trained on very very huge and variable dataset, which should be very generic and tried to "fine-tune" (hope not overfitted much)