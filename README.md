# Personal Repo
## Learning Tensorflow.

# Notes

- Command to run GPU tensorflow with the volume mounted:
```bash
nvidia-docker run \
-v /media/ashwin/6484DC2984DC000A/tensorflow-gpu:/home \
-it -p 8888:8888 gcr.io/tensorflow/tensorflow:latest-gpu \
/bin/bash
```
- `MNIST_data` is gitignored.
