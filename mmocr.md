## <u>MMOCR</u>

#### Installation Steps

##### Step 1: Install Packages

```
pip install -U openmim
mim install "mmengine>=0.7.1,<1.1.0"
mim install "mmcv>=2.0.0rc4,<2.1.0"
mim install "mmdet>=3.0.0rc5,<3.2.0"
```

##### Step 2: Clone the MMOCR Repository

```
conda activate pick
cd PICK-pytorch
git clone https://github.com/open-mmlab/mmocr.git
cd mmocr
pip install -v -e .
```

##### Quick Run

Create a python script to run the following code.

```
from mmocr.apis import MMOCRInferencer
# Load models into memory
ocr = MMOCRInferencer(det='DBNetpp', rec='ABINet')
# Perform inference
ocr('new.jpg',out_dir='outputs_ocr/', save_pred=True, save_vis=True)
```
