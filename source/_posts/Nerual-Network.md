---
title: Nerual-Network
date: 2021-04-30 10:06:39
tags:
---

## MLP

```python
import numpy
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.utils import np_utils
# fix random seed for reproducibility

seed = 7
numpy.random.seed(seed)
# load data
(X_train, y_train), (X_test, y_test) = mnist.load_data()
# flatten 28*28 images to a 784 vector for each image
num_pixels = X_train.shape[1] * X_train.shape[2]
X_train = X_train.reshape(X_train.shape[0], num_pixels).astype('float32')
X_test = X_test.reshape(X_test.shape[0], num_pixels).astype('float32')
# normalize inputs from 0-255 to 0-1
X_train = X_train / 255
X_test = X_test / 255
# one hot encode outputs
y_train = np_utils.to_categorical(y_train)
y_test = np_utils.to_categorical(y_test)
num_classes = y_test.shape[1]
# define baseline model


def baseline_model():
    # create model
    model = Sequential()

    # default value--init is error(exception)
    # model.add(Dense(num_pixels, input_dim=num_pixels, init='normal', activation='relu'))
    # model.add(Dense(num_classes, init='normal', activation='softmax'))

    model.add(Dense(num_pixels, input_dim=num_pixels, activation='relu'))
    model.add(Dense(num_classes, activation='softmax'))

    # Compile model
    model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
    return model


# build the model
model = baseline_model()
# Fit the model
model.fit(X_train, y_train, validation_data=(X_test, y_test), epochs=10, batch_size=200, verbose=2)
# Final evaluation of the model
scores = model.evaluate(X_test, y_test, verbose=0)
print("MLP Error: %.2f%%" % (100-scores[1]*100))
```

## CNN

`Convolution Neural Network`

卷积神经网络主要有三层构成包括：卷积层、线性整流层和池化层。完整的神经网络结构有以下构成，包括输入层（input layer）、卷积层（Convolutional Layer）、线性整流层（Rectified Linear Units Layer）又称activation function--Relu、池化层（Pooling Layer）又称（Down sampling Layer）、全连接层（Full Connected Layer）、输出层（output layer）。

The convolutional layer can turn the original data(for image) into feature map by convolutional kernel. Convolution Kernel designed by the mechanical learning and modified by the full connected layer based on back propagation algorithm.

```python
import numpy
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers import Flatten
from keras.layers.convolutional import Convolution2D
from keras.layers.convolutional import MaxPooling2D
from keras.utils import np_utils
from keras import backend as K

# fix random seed for reproducibility

seed = 7
numpy.random.seed(seed)

# load data
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# reshape to be [samples][pixels][width][height]
X_train = X_train.reshape(X_train.shape[0], 28, 28, 1).astype('float32')
X_test = X_test.reshape(X_test.shape[0], 28, 28, 1).astype('float32')

# normalize inputs from 0-255 to 0-1
X_train = X_train / 255
X_test = X_test / 255

# one hot encode outputs
y_train = np_utils.to_categorical(y_train)
y_test = np_utils.to_categorical(y_test)
num_classes = y_test.shape[1]


def baseline_model():
   # create model
   model = Sequential()
   model.add(Convolution2D(32, (5, 5), padding='valid', input_shape=(28, 28, 1), activation='relu'))
   model.add(MaxPooling2D(pool_size=(2, 2)))
   model.add(Dropout(0.2))
   model.add(Flatten())
   model.add(Dense(128, activation='relu'))
   model.add(Dense(num_classes, activation='softmax'))
   # Compile model
   model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
   return model


# build the model
model = baseline_model()
# Fit the model
model.fit(X_train, y_train, validation_data=(X_test, y_test), epochs=10, batch_size=200, verbose=2)
# Final evaluation of the model
scores = model.evaluate(X_test, y_test, verbose=0)
print("CNN Error: %.2f%%" % (100-scores[1]*100))
```

## Enhancement CNN

```python
import numpy
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers import Flatten
from keras.layers.convolutional import Convolution2D
from keras.layers.convolutional import MaxPooling2D
from keras.utils import np_utils
from keras import backend as K

# fix random seed for reproducibility

seed = 7
numpy.random.seed(seed)

# load data
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# reshape to be [samples][pixels][width][height]
X_train = X_train.reshape(X_train.shape[0], 28, 28, 1).astype('float32')
X_test = X_test.reshape(X_test.shape[0], 28, 28, 1).astype('float32')

# normalize inputs from 0-255 to 0-1
X_train = X_train / 255
X_test = X_test / 255

# one hot encode outputs
y_train = np_utils.to_categorical(y_train)
y_test = np_utils.to_categorical(y_test)
num_classes = y_test.shape[1]


def larger_model():
   # create model
   model = Sequential()
   model.add(Convolution2D(30, (5, 5), padding='valid', input_shape=(28, 28, 1), activation='relu'))
   model.add(MaxPooling2D(pool_size=(2, 2)))
   model.add(Convolution2D(15, 3, 3, activation='relu'))
   model.add(MaxPooling2D(pool_size=(2, 2)))
   model.add(Dropout(0.2))
   model.add(Flatten())
   model.add(Dense(128, activation='relu'))
   model.add(Dense(50, activation='relu'))
   model.add(Dense(num_classes, activation='softmax'))

   # Compile model
   model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
   return model


# build the model
model = larger_model()

# Fit the model
model.fit(X_train, y_train, validation_data=(X_test, y_test), epochs=10, batch_size=200, verbose=2)

# Final evaluation of the model
scores = model.evaluate(X_test, y_test, verbose=0)
print("Larger CNN Error: %.2f%%" % (100-scores[1]*100))
```

