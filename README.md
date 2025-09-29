# Wyoming Whisper.cpp

[Wyoming protocol](https://github.com/rhasspy/wyoming) server for the [whisper.cpp](https://github.com/ggerganov/whisper.cpp) speech to text system.

## Local Install

Install dependencies:

Ubuntu
```sh
sudo apt-get install build-essential
```

Fedora
```sh
sudo dnf install gcc gcc-c++
```

Clone the repository and set up Python virtual environment:

``` sh
git clone https://github.com/rhasspy/wyoming-whisper-cpp.git
cd wyoming-whisper-cpp
script/setup
```

Build the whisper.cpp `whisper-server` executable:

```sh
cmake -S whisper.cpp -B whisper.cpp/build
cmake --build whisper.cpp/build -j --config Release
```

### GPU Support

Whisper.cpp also supports the utilisation of GPUs which will greatly accelerate speech synthesis. Supported is a CUDA backend for NVIDIA GPUs and and a Vulkan backend which can be used on almost all GPUs:

Vulkan is a cross-vendor solution which allows you to accelerate workload on your GPU. First, make sure your graphics card driver provides support for Vulkan API.
Then use the following commands to add GPU acceleration using **Vulkan**:
```sh
cmake -S whisper.cpp -B whisper.cpp/build -DGGML_VULKAN=1
cmake --build whisper.cpp/build -j --config Release
```

With NVIDIA cards the processing of the models is done efficiently on the GPU via cuBLAS and custom CUDA kernels. First, make sure you have installed cuda: https://developer.nvidia.com/cuda-downloads

Then use the following commands to add GPU acceleration using **CUDA**:
```sh
cmake -S whisper.cpp -B whisper.cpp/build -DGGML_CUDA=1
cmake --build whisper.cpp/build -j --config Release
```

or for newer NVIDIA GPU's (RTX 5000 series):
```sh
cmake -S whisper.cpp -B whisper.cpp/build -DGGML_CUDA=1 -DCMAKE_CUDA_ARCHITECTURES="86"
cmake --build whisper.cpp/build -j --config Release
```

Run a server anyone can connect to:
```sh
script/run \
  --whisper-cpp-dir ./whisper.cpp \
  --model tiny.en \
  --language en \
  --uri 'tcp://0.0.0.0:10300' \
  --data-dir /data \
  --download-dir /data
```

## Docker Image

``` sh
docker run -it -p 10300:10300 -v /path/to/local/data:/data rhasspy/wyoming-whisper-cpp \
    --data-dir /data --model tiny.en --language en
```

[Source](https://github.com/rhasspy/wyoming-addons/tree/master/whisper-cpp)

## Advanced

For more advanced use cases, consider the community-built [wyoming-whisper-api-client](https://github.com/ser/wyoming-whisper-api-client)
