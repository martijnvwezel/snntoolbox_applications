# Examples

To set up an experiment with the SNN toolbox, we need to create a config
file as described in [the
documentation](https://snntoolbox.readthedocs.io/en/latest/guide/configuration.html).
Here we show a number of examples of such configuration files.

> * A.  [LeNet on MNIST](#-Example-A---LeNet-on-MNIST)
> * B.  [BinaryConnect on CIFAR-10](#-Example-B---BinaryConnect-on-CIFAR-10)
> * C.  [BinaryNet on CIFAR-10](#-Example-C---BinaryNet-on-CIFAR-10)
> * D.  [VGG-16 on ImageNet](#-Example-D---VGG-16-on-ImageNet)
> * E.  [Inception-V3 on ImageNet](#-Example-E---Inception-V3-on-ImageNet)

These example applications contain the network architectures,
parameters, the configuration files for running the toolbox, as well as
a subset of the data sets. You can simply run them by typing
`snntoolbox -t <path_to_config>` in the terminal, where
`<path_to_config>` is the path (including filename) to the configuration
file corresponding to that particular experiment.

#### Note
In order to successfully run the example models here, make sure the
[Keras configuration file](https://keras.io/getting-started/faq/#where-is-the-keras-configuration-file-stored)
contains the option `"image_data_format": "channels_first"`.


## Example A - LeNet on MNIST


Here we test the classic LeNet architecture on MNIST, using one
implementation in Keras, one in Lasagne, and one in Caffe.

### Keras

The most important part to specify in the `config` file is the
`filename_ann` and `dataset_path` parameter in the `paths` section. This
will tell the toolbox where to look for the input model and data.

It is possible to leave some settings like `path_wd` open; they will be
filled in with the default values. For instance, `path_wd` is set to the
directory where the `config` file lives.

Note how we can specify relative paths, or even refer to other options
(like `path_wd`) when building other paths. If only a filename (without
path) is given, or the path is relative, it is always assumed to be
relative to the `path_wd` option.

In the `simulation` section we tell the toolbox how long and how many
samples to test.

Optionally, we can ask the toolbox to `output` plots and save some
quantities that were monitored during the simulation to disk.

#### Note

The model `98.96.h5` in the example above was trained using Keras
version \<= 2.1.6. If you installed the toolbox using a newer Keras
version, this model may show a drop in accuracy because of a change in
the Flatten layer. Downgrade Keras to maintain accuracy, or set
`filename_ann = 99.14` to use a model trained with Keras 2.2.4.


### Caffe

You need to have Caffe installed to run this example.

Here we need to change the `model_lib` option from default `keras` to
`caffe`. The `filename_ann` also changes, the rest can stay the same.

### Lasagne

You need to have Lasagne installed to run this example.

We can also use Poisson input as shown below:

## Example B - BinaryConnect on CIFAR-10

You need to have Lasagne installed to run this example.

The network has been trained with binary weights and can be tested using
either full-precision or binary weights. Set the `binarize_weights`
option accordingly.

## Example C - BinaryNet on CIFAR-10

You need to have Lasagne installed to run this example.

This network is like BinaryConnect, but in addition to binary weights
also uses binary activations.

Note how we turn off `normalize` in `tools`. Parameter normalization is
not required here because activations never exceed threshold anyways.

## Example D - VGG-16 on ImageNet

This example shows how to use `jpg` images as `dataset_format`. Options
that are needed for this include `class_idx_path`, `datagen_kwargs`, and
`dataflow_kwargs`.

Further, we use a membrane clamp to reduce transient neuron dynamics in
the beginning of the simulation. For this, we set the
`filename_clamp_indices` option.

For memory reasons we do not include the model file here, but the
example should work when instantiating VGG-16 from the Keras model zoo.

## Example E - Inception-V3 on ImageNet

If we want the Keras.ImageDataGenerator to perform some kind of
preprocessing on the data, we need to pass a `preprocessing_function` to
the `datagen_kwargs`. This can be done by simply giving the name of a
python module containing the preprocessing function. The toolbox will
import and use the function from there.
