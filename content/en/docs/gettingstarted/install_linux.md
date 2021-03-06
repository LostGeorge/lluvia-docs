---
title: "Install Linux"
date: 2020-08-07T00:00:00-05:00
weight: 1
---


# Installation (Ubuntu 18.04)


## Dependencies


Install the following packages in your system if they are still not available:

```bash
apt install \
    build-essential \
    cmake \
    clang
```

Install Bazel:

```bash
curl https://bazel.build/bazel-release.pub.gpg | apt-key add -
echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list
apt update
apt install bazel
```

Install the [LunarG Vulkan SDK](https://www.lunarg.com/vulkan-sdk/).

```bash
wget -qO - http://packages.lunarg.com/lunarg-signing-key-pub.asc >> lunarg-signing-key-pub.asc
apt-key add lunarg-signing-key-pub.asc
wget -qO /etc/apt/sources.list.d/lunarg-vulkan-1.1.121-bionic.list http://packages.lunarg.com/vulkan/1.1.121/lunarg-vulkan-1.1.121-bionic.list
apt-get update 
apt-get install lunarg-vulkan-sdk
```

Verify that the SDK was successfully installed by running:

```bash 
vulkaninfo
```


## Build C++ Libraries

Clone and compile Lluvia's C++ libraries:

```bash
git clone https://github.com/jadarve/lluvia.git

cd lluvia
CC=clang bazel build //lluvia/cpp/...
```

Run the tests to verify that your compilation runs properly:

```bash
CC=clang bazel test //lluvia/cpp/...
```


## Python3 package

To build the Python3 package, execute the commands below from the repository's top-level directory. You can create a [virtual environment](https://virtualenv.pypa.io/en/latest/) to isolate the installation:

```bash
CC=clang bazel build //lluvia/python:lluvia_wheel
CC=clang bazel test //lluvia/test/...
pip3 install bazel-bin/lluvia/python/lluvia-0.0.1-py3-none-any.whl
```

Open a Python3 interpreter and import lluvia package

```python 
import lluvia as ll
```

If the import completes successfully, lluvia is ready for use.
