## Image Caption Generator

A neural network to generate captions for an image using CNN and RNN with BEAM Search and then using ViT + GPT2


## Table of Contents

1. [Requirements](#1-requirements)
2. [Training parameters and results](#2-training-parameters-and-results)
3. [Generated Captions on Test Images](#3-generated-captions-on-test-images)
4. [Procedure to Train Model](#4-procedure-to-train-model)
5. [Procedure to Test on new images](#5-procedure-to-test-on-new-images)
6. [Configurations (config.py)](#6-configurations-configpy)
7. [Frequently encountered problems](#7-frequently-encountered-problems)
8. [TODO](#8-todo)
9. [References](#9-references)

## 1. Requirements

Recommended System Requirements to train model.

<ul type="square">
	<li>A good CPU and a GPU with atleast 8GB memory</li>
	<li>Atleast 8GB of RAM</li>
	<li>Active internet connection so that keras can download inceptionv3/vgg16 model weights</li>
</ul>

Required libraries for Python along with their version numbers used while making & testing of this project

<ul type="square">
	<li>Python - 3.6.7</li>
	<li>Numpy - 1.16.4</li>
	<li>Tensorflow - 1.13.1</li>
	<li>Keras - 2.2.4</li>
	<li>nltk - 3.2.5</li>
	<li>PIL - 4.3.0</li>
	<li>Matplotlib - 3.0.3</li>
	<li>tqdm - 4.28.1</li>
</ul>


<ul type="square">
	<li><a href="https://github.com/jbrownlee/Datasets/releases/download/Flickr8k/Flickr8k_Dataset.zip">Flickr8k_Dataset</a></li>
	<li><a href="https://github.com/jbrownlee/Datasets/releases/download/Flickr8k/Flickr8k_text.zip">Flickr8k_text</a></li>
</ul>

<strong>Important:</strong> After downloading the dataset, put the reqired files in train_val_data folder

## 2. Training parameters and results

#### NOTE

- `batch_size=64` took ~14GB GPU memory in case of *InceptionV3 + AlternativeRNN* and *VGG16 + AlternativeRNN*
- `batch_size=64` took ~8GB GPU memory in case of *InceptionV3 + RNN* and *VGG16 + RNN*
- **If you're low on memory**, use google colab or reduce batch size
- In case of BEAM Search, `loss` and `val_loss` are same as in case of argmax since the model is same


<img width="607" alt="Screenshot 2023-12-07 at 8 57 19 AM" src="https://github.com/sruthik11/Image-Caption-Generation/assets/39257137/95b66880-6dc2-45e0-a53e-a22c8b263ce2">
<img width="603" alt="Screenshot 2023-12-07 at 9 12 03 AM" src="https://github.com/sruthik11/Image-Caption-Generation/assets/39257137/eee782c7-731a-4547-a98e-5cacc741d5e3">


## 3. Generated Captions on Test Images
<img width="548" alt="Screenshot 2023-12-07 at 9 26 28 AM" src="https://github.com/sruthik11/Image-Caption-Generation/assets/39257137/057292e9-e629-4c9d-aedf-f7ab91b271fa">


## 4. Procedure to Train Model

1. Clone the repository to preserve directory structure.<br>
`git clone https://github.com/sruthik11/Image-Caption-Generator.git`
2. Put the required dataset files in `train_val_data` folder (files mentioned in readme there).
3. Review `config.py` for paths and other configurations (explained below).
4. Run `train_val.py`.

## 5. Procedure to Test on new images

1. Clone the repository to preserve directory structure.<br>
`git clone https://github.com/sruthik11/Image-Caption-Generator.git`
2. Train the model to generate required files in `model_data` folder (steps given above).
3. Put the test images in `test_data` folder.
4. Review `config.py` for paths and other configurations (explained below).
5. Run `test.py`.

## 6. Configurations (config.py)

**config**

1. **`images_path`** :- Folder path containing flickr dataset images
2. `train_data_path` :- .txt file path containing images ids for training
3. `val_data_path` :- .txt file path containing imgage ids for validation
4. `captions_path` :- .txt file path containing captions
5. `tokenizer_path` :- path for saving tokenizer
6. `model_data_path` :- path for saving files related to model
7. **`model_load_path`** :- path for loading trained model
8. **`num_of_epochs`** :- Number of epochs
9. **`max_length`** :- Maximum length of captions. This is set manually after training of model and required for test.py
10. **`batch_size`** :- Batch size for training (larger will consume more GPU & CPU memory)
11. **`beam_search_k`** :- BEAM search parameter which tells the algorithm how many words to consider at a time.
11. `test_data_path` :- Folder path containing images for testing/inference
12. **`model_type`** :- CNN Model type to use -> inceptionv3 or vgg16
13. **`random_seed`** :- Random seed for reproducibility of results

**rnnConfig**

1. **`embedding_size`** :- Embedding size used in Decoder(RNN) Model
2. **`LSTM_units`** :- Number of LSTM units in Decoder(RNN) Model
3. **`dense_units`** :- Number of Dense units in Decoder(RNN) Model
4. **`dropout`** :- Dropout probability used in Dropout layer in Decoder(RNN) Model

## 7. Frequently encountered problems

- **Out of memory issue**:
  - Try reducing `batch_size`
- **Results differ everytime I run script**:
  - Due to stochastic nature of these algoritms, results *may* differ slightly everytime. Even though I did set random seed to make results reproducible, results *may* differ slightly.
- **Results aren't very great using beam search compared to argmax**:
  - Try higher `k` in BEAM search using `beam_search_k` parameter in config. Note that higher `k` will improve results but it'll also increase inference time significantly.


## 9. References

<ul type="square">
	<li><a href="https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Vinyals_Show_and_Tell_2015_CVPR_paper.pdf">Show and Tell: A Neural Image Caption Generator</a> - Oriol Vinyals, Alexander Toshev, Samy Bengio, Dumitru Erhan</li>
	<li><a href="https://arxiv.org/abs/1703.09137">Where to put the Image in an Image Caption Generator</a> - Marc Tanti, Albert Gatt, Kenneth P. Camilleri</li>
	<li><a href="https://machinelearningmastery.com/develop-a-deep-learning-caption-generation-model-in-python/">How to Develop a Deep Learning Photo Caption Generator from Scratch</a></li>
</ul>
