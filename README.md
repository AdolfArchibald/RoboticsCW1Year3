# Robotics Coursework 1

Please note that this project was done on **Python 3.11.9** in **VSCode**.

Your code will NOT work unless you use Python version **3.8 - 3.11**.

Additionally, you will need **Jupyter Notebook Support**.

Kindly install Jupyter Notebook from the VSCode extensions page.

## Required Pip Installs

If you are running multiple versions of Python, use the following commands:

```bash
pip3.11 install tensorflow
pip3.11 install tf-keras
pip3.11 install pillow
pip3.11 install matplotlib
```

# Data Source

You can download the data source from the following link:

https://drive.google.com/drive/folders/1X5sp0sMgKHqmEL_L0LwQUzv06cFPft7M?usp=sharing

Please make sure to unzip the file and place it into the root folder, at the same level as the code folder. NOT in the code folder.


# Code Overview

Below you will find a brief overview of all the steps taken to get the working model.

## Preprocessing

The **Preprocessing** step involves preparing the raw data before feeding it into the model. This includes:

## Function: `preprocess_and_save_images`

Loads images from a directory, resizes them to a target size, normalizes the pixel values, and encodes the labels as integers. It returns the preprocessed images as a NumPy array, the encoded labels, and the unique class labels.

- This is applied to all the directories in the data folder. These directories are used as **classes** by the model.

- Next, all images and labels are combined in preperation for feeding into the model.

These steps ensure that the input data is in the right format for training and help improve the model's performance.

---

## Train / Test Split

In the **Train / Test Split** section, the dataset is divided into two parts:

- **Training Set**: A portion of the data used to train the model. We used 80% of the data for the training set.
- **Test Set**: A separate portion of the data used to evaluate the model's performance after training. We used 20% of the data.

This split ensures that the model is evaluated on unseen data, which helps to prevent overfitting and provides a more accurate measure of the model’s ability to generalize to new, unseen data.

---

## Creating Datasets

The **Creating Datasets** section involves organizing the data into a format that can be easily consumed by the model.

## Function: `create_dataset`

Converts the input images and labels into a TensorFlow `Dataset`, shuffling the data, batching it by the specified `batch_size`, and prefetching for optimized performance during training.

- **Create Dataset Function** is then applied to the train and test datasets.

## Function: `custom_loss`

- **Custom Loss Function**: Computes sparse categorical cross-entropy loss while ignoring the labels with a value of `-1`, by masking out those values before calculating the loss.


This ensures that the model gets properly structured input data during training and evaluation.

---

## Building the Model

The **Building the Model** section defines the architecture of the neural network.

## Model Architecture

## Model Architecture

- **Input Layer**: We have an input layer with shape `(300, 300, 3)` for 300x300 RGB images.
- **Convolutional Layers**: We have 3 convolutional layers with 32, 64, and 128 filters of size `(3, 3)`, each followed by a max pooling layer with a pool size of `(2, 2)` to extract features and reduce spatial dimensions.
- **Flatten Layer**: We flatten the output of the last convolutional layer into a 1D vector for the dense layers.
- **Fully Connected Layer**: We use a dense layer with 256 units and a ReLU activation to learn complex patterns.
- **Dropout Layer**: We apply a dropout rate of 0.5 to prevent overfitting during training.
- **Output Layers**: We specifiy the output layers.

The model architecture was chosen based on the **Uno Card Problem** being solved and involves several layers stacked together to capture increasingly complex features from the image data.

---

## Defining, Compiling, and Training the Model

- **Optimizer**: We use the Adam optimizer with a learning rate of 0.001 for efficient training.
- **Loss Function**: We apply a custom loss function (`custom_loss`) for each output (`color_output`, `number_output`, and `special_output`) to handle class imbalance and focus on relevant classes.
- **Metrics**: We track accuracy for each output during training to evaluate the model’s performance in predicting color, number, and special card labels.

Finally, the model is trained for **30 epochs** as we found this to be where the diminishing returns of the training process are too great to yield any better results for accuracy.

---

## Model Metrics

A small section where we make use of **mathplotlib** to plot a graph to show training and validation accuracy for **each metric**.

---

## Prediction Functions

The **Prediction Functions** section defines how the model is used to make predictions on new, unseen data. This involves 2 functions.

## Function: `pedict_uno_card`

**predict_uno_card** takes a `path/to/the/image`, and is used for when the user wants to upload an image to make a prediction on.

## Function: `pedict_uno_card_with_img`

**predict_uno_card** takes an `Pillow image object` (normal image) directly to make a prediction.

These function are called when the model is used in the GUI to make real-time predictions on live or uploaded data.

---

## GUI (Graphical User Interface)

The **GUI** section involves creating a user-friendly interface to interact with the model. It includes:

- **File upload functionality** to allow users to select an image for prediction.
- **Display components** labels and images to show the prediction results in real-time.
- **Live streaming** option for interactive or continuous prediction use cases.
- **Buttons** for uploading an imag or starting the livestream.

The GUI makes it easy for non-technical users to interact with the trained model and see results without needing to interact directly with the code.

---
