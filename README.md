# Compressed Sensing with Deep Image Prior

This repository provides code to reproduce results from the paper: [Compressed Sensing with Deep Image Prior and Learned Regularization](https://arxiv.org/pdf/1806.06438.pdf). Functionality learned regularization method of this paper is currently in progress.

Here are a few example results:
<img src="https://github.com/davevanveen/compsensing_dip/blob/master/reconstructions/mnist/mnist_sample_100meas.pdf" alt="mnist_reconstr" width="300">


## Steps to reproduce the results
Note: Please run **all** commands from the root directory of the repository, i.e from ```csgm/```

### Preliminaries
---

1. Clone the repository
    ```shell
    $ git clone https://github.com/davevanveen/compsensing_dip.git
    $ cd csgm??
    ```
2. Install requirements
    ```shell
    $ pip install -r requirements.txt
    ```
    Need to include list of requirements?

### Generate plots/reconstructions with existing data
---
Recreate plots of pre-generated reconstructions on MNIST and x-ray datasets in jupyter notebook
1. Open jupyter notebook of plots
    ```shell
    $ jupyter notebook plot.ipynb
    ```	
2. Set the variables ```DATASET``` and ```NUM_MEASUREMENTS LIST``` to plot reconstructions of interest

### Generating new reconstructions on the MNIST or xray datasets
---
1. Execute the baseline command
	```shell
	$ python comp_sensing.py
	```
	which will run experiments with the default parameters specified in ```configs.json```

2. To create reconstructions according to user specified parameters, add command line arguments according to ```parser.py```. Example:
	```shell
	$ python comp_sensing.py --DATASET xray --NUM_MEASUREMENTS 2000 4000 8000 --BASIS csdip dct wavelet --NUM_ITER 500
	```
Note: To reduce runtime, redundant reconstructions will not be generated for the same image at the same number of measurements. Thus if you wish to compare reconstructions while varying other parameters (e.g. learning rate, weight decay), your data must be manually saved to avoid file overwrite.

### Running CS-DIP on a new dataset
---
1. Create a new directory ```/data/dataset_name/sub/``` which contains your images
2. In ```utils.py```, create a new DCGAN architecture. This will be similar to the pre-defined ```DCGAN_MNIST``` and ```DCGAN_XRAY``` but must have output dimension according to the size of the new images. This can be accomplished by adjusting kernel_size, stride, and padding as discussed in [torch.nn documentation](https://pytorch.org/docs/stable/nn.html#convtranspose2d). 
3. Update ```configs.json```, ```parser.py``` to set parameters for your dataset. Update ```cs_dip.py``` to import/initiate the corresponding DCGAN.
4. Generate and plot reconstructions according to instructions above.
Note: We recommend experimenting with the DCGAN architecture and dataset parameters to obtain the best possible reconstructions.




