# Personal Repo
## Learning Tensorflow.

## To run it with the mount
- go to the location you want to run the file from
- open terminal
- enter
```bash
nvidia-docker run -v $(pwd):/home -it -p 8888:8888 ashwinning/tensorflow:0.1 /bin/bash
```

# Notes

- `MNIST_data` is gitignored.

----

# FUN FACT
The tensorflow-gpu docker image (gcr.io/tensorflow/tensorflow:latest-gpu) is not correctly configured with cuDNN which is **BULLSHIT**. (as of July 11, 2016)

Runing the multilayer convolution will give you the following error
```
tensorflow/stream_executor/cuda/cuda_dnn.cc:204] could not find cudnnCreate in cudnn DSO;
dlerror: /usr/local/lib/python2.7/dist-packages/tensorflow/python/_pywrap_tensorflow.so:
undefined symbol: cudnnCreate
```

To make it worse, [Tensorflow docs](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#optional-install-cuda-gpus-on-linux) ask you to install cuDNN 4.5, which is fine, but their example code is as follows
```bash
tar xvzf cudnn-7.5-linux-x64-v4.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

Note the `cudnn-7.5-linux-x64-v4.tgz`?

Yeah, so turns out I'm an idiot.

When you go to the cuDNN download website, there isn't really a link for `cudnn-7.5-linux-x64-v4` - there's a cuDNN v5 for toolkit 7.5 and a v4 for toolkit 7.0

![](https://i.gyazo.com/5b424f2577b550522788cd140b6b1d5c.png)

And since I'm an idiot, I kept installing `cudnn-7.5-linux-x64-v5.0-ga.tgz`.

Which gave me a different error:
```
tensorflow/stream_executor/cuda/cuda_dnn.cc:427] could not set cudnn filter descriptor:
CUDNN_STATUS_BAD_PARAM
```

Turns out, v4 is good for 7.0 **AND LATER**

![](https://i.gyazo.com/86d8b0aaff1d40054403c08a19389da4.png)

So yeah, installing that into the docker machine worked.

Also, `sudo cp cuda/include/cudnn.h /usr/local/cuda/include` will not give you an error if an `include` folder does not exist - **which is also bullshit**.

Turned out a file called include existed there.

Forcefully creating a directory there helped the situation.
