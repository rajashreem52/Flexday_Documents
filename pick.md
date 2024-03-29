## <u>PICK </u>

### Requirements

- Python = 3.8.10
- torchvision =  0.14.1+cu116
- tabulate = 0.8.7
- overrides = 3.0.0
- opencv_python = 4.9.0.80
- opencv-python-headless = 4.9.0.80
- numpy =  1.22.0
- pandas = 1.0.5
- allennlp = 1.0.0
- torchtext = 0.6.0
- tqdm = 4.65.2
- torch = 1.13.1+cu116

## Installation Steps

#### Step 1: Create a Virtual Environment

```
conda create --name pick python=3.8.10

conda activate pick
```

#### Step 2: Clone the PICK Repository

```
git clone https://github.com/rajashreem52/PICK_EASYOCR.git
```

#### Step 3: Install Required Packages

Install the required packages using pip, and then proceed to install the latest stable version of PyTorch and torchvision that is compatible with your CUDA version. Use the following code to install the latest stable releases.

```
 pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 -f https://download.pytorch.org/whl/cu116/torch_stable.html
```

### Training

#### Step 1: Prepare the Dataset

```
cd PICK-pytorch
```

Check the details of the data format from the following link.

```
https://github.com/wenwenyu/PICK-pytorch/tree/master/data
```

Save the dataset in the "PICK-pytorch" folder.  An example of the dataset format is given below. 

```
├── boxes_and_transcripts
│   ├── Image_0.tsv
│   └── Image_1.tsv
└── images
    ├── Image_0.jpg
    └── Image_1.jpg  
├── test.csv
├── train.csv
```

#### Step 2: Modify  Configuration

- Modify `train_dataset` and `validation_dataset` args in `config.json` file, including `files_name`, `images_folder`, `boxes_and_transcripts_folder`, `entities_folder`, `iob_tagging_type` and `resized_image_size`.

- Modify `Entities_list` in `utils/entities_list.py` file according to the entity type of your dataset.

- Modify `keys.txt` in `utils/keys.txt` file if needed according to the vocabulary of your dataset.

- Modify `MAX_BOXES_NUM` and `MAX_TRANSCRIPT_LEN` in `data_tuils/documents.py` file if needed.

#### Step 3: Training Script

```
CUDA_VISIBLE_DEVICES=0,1 python3 -m torch.distributed.launch --nnodes=1 --node_rank=0 --nproc_per_node=2 --master_addr=127.0.0.1 --master_port=5555 train.py -c config.json --local_world_size 2
```

### Testing

```
python test.py --checkpoint path/to/checkpoint --boxes_transcripts path/to/boxes_transcripts \
               --images_path path/to/images_path --output_folder path/to/output_folder \
               --gpu 0 --batch_size 2
```

Replace the checkpoint, image_path folder, and output folder. The models are automatically saved in the /PICK-pytorch/saved/models/PICK_Default folder. An example of a testing script is given below.

```
 python3 test.py --checkpoint ~/PICK-pytorch/saved/models/PICK_Default/test_1221_004907/model_best.pth --boxes_transcripts ~/PICK-pytorch/boxx --images_path  ~/PICK-pytorch/images --output_folder ~/PICK-pytorch/passport_outputs --gpu 0 --batch_size 2
```

The results are saved in the "passport_outputs" folder. Each image's keys and values are recorded in a corresponding "<Image_file_name>.txt" file. 

### Inference

##### Step 1:  Install the required packages

```
pip install easyocr
pip install opencv-python
```

##### Step 2:  Run the Prediction Script

```
bash predict.sh
```

The resulting output will be as follows. When conducting inference with a single image, you may input the image filename. For multiple-image inference, input the image folder. However, avoid providing both simultaneously. You have the option to include a preferred checkpoint, or the script will use the default value. Similarly, you can specify an output folder name, or the script will default to the provided default folder called "output_folder".

```
Usage: predict.sh (--image_filename <image_filename> | --image_folder <image_folder>) [--output_folder <output_folder>] [--checkpoint <checkpoint_path>]
```

An example usage for a single image inference is given below:

```
 bash predict.sh --image_filename new.jpg
```

The generated output will be stored in the default output folder under the filename "new.txt". Therefore, the output file will adopt the name of the input image filename (excluding the extension) followed by ".txt".
