# <u>Generate Synthetic Passport Data</u>

#### Installation Steps

###### Required Packages

Python version: 3.10.4

```
pip install Pillow

pip install nump

pip install opencv-python

pip install faker
```

###### Download Code

```
git clone https://github.com/rajashreem52/Passport_Data_Generator.git
```

#### Quick Run

###### Generate Synthetic data

Run passport_data_generator.py to generate fake passports. 

```
cd Passport_Data_Generator

py passport_data_generator.py -n <number_of_passports> -o <output_file_name>.json
```

Executing the following code will generate 10,000 sets of passport information and store the data in a file named fake_passports.json

```
cd Passport_Data_Generator

py passport_data_generator.py -n 10000 -o fake_passports.json
```

###### Generate Dataset for training PICK

###### Step 1: Generate the Dataset

Execute the main.py script to generate the dataset, and optionally include the <-t> flag to produce transformed images. The generated data will be formatted in PICK format.

```
python main.py -n <number_of_passports> -o <dataset_folder_name> -t(optional)
```

Running the following code will create 5 passport images and their corresponding annotation files, which will be saved in a file named `<imag_file_name>.tsv`. Including the `-t` option will additionally generate transformed **(perspective transformation) **versions of the images and store their corresponding annotation files. 

```
python main.py -n 5 -o output_folder -t
```

The output of the above code is a folder named "output_folder" which has the following format. The folder will contain a folder named "images" which will contain all the generated images and annotations will be saved in the "boxes_trans" folder.

So, the dataset has the following format.

```
dataset
├───boxes_trans
└───images
```

###### Step 2: Split the Dataset

Execute the train_test.py script to divide the dataset into training and testing subsets. Running this script will produce two files, train.csv, and test.csv, within the dataset_folder.

```
 py test_train.py <dataset_folder_name>
```
