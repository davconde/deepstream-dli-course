# deepstream-dli-course
Running the official NVIDIA Deep Learning Institute DeepStream course without any Jetson.

## Setup
Download the official Docker image for DeepStream 7.0:
```bash
sudo docker pull nvcr.io/nvidia/deepstream:7.0-triton-multiarch
```

In case of running the container on WSL, follow [these instructions](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_on_WSL2.html). Afterwards, install specific X11 utils:
```bash
sudo apt install x11-xserver-utils
```

Run the container with ports and display overrides, GPU support and virtual mounts:
```bash
xhost +
sudo docker run -it --privileged --rm --name=docker --net=host --gpus all -e DISPLAY=$DISPLAY -e CUDA_CACHE_DISABLE=0 --device /dev/snd -v /tmp/.X11-unix/:/tmp/.X11-unix -v ~/:/opt/nvidia/deepstream/deepstream-7.0/my_home/ -p 8554:8554 nvcr.io/nvidia/deepstream:7.0-triton-multiarch
```
This will create in the working directory a link named `my_home` to the user home of the host, where the [course files](https://universidadevigo-my.sharepoint.com/:u:/g/personal/david_conde_morales_uvigo_gal/EdpVaRnXfPNGr5kKAGT8CwYBnDhX-VJEMzqNkYa-v7Qy9Q?e=c7P2Du) should be located and extracted.

Install the additional Python based applications:
```bash
./user_deepstream_python_apps_install.sh --version 1.1.11
```

## Running Jupyter in Visual Studio Code

In Visual Studio Code install the Docker extension, then in the Docker sidebar right click on the executing container and select `Attach Visual Studio Code`.
In the new window connected to the remote, install both the Python and Jupyter extensions as local extensions, which will manage the Jupyter kernel lifecycle.

## Running Jupyter in web browser

Although Visual Studio Code is recommended for easier code edition during this course, Jupyter notebooks can be edited in a web browser.

Install the Jupyter runtime and webservice, then launch it:
```bash
pip install jupyterlab
pip install notebook
jupyter notebook --allow-root
```

To access the Jupyter web view without forwarding the Jupyter port and using a browser in host, install one in the container.
For this, open a new terminal instance, get the container ID with `docker ps`, and attach it with `docker attach <container-id>`. Then run:
```bash
add-apt-repository -y ppa:savoury1/chromium
apt update
apt install -y chromium-browser
chromium-browser --no-sandbox
```

In the web browser, enter the URL indicated by the Jupyter server console output.

## Video output

When running tests that display image, open VLC and in `Media>Open Network Stream...` introduce:
```shell
rtsp://localhost:8554/ds-test
```
