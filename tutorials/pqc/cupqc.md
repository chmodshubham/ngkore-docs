# cuPQC

**Authors:** [Lakshya Chopra](https://www.linkedin.com/in/lakshyachopraa/), [Satyam Dubey](https://www.linkedin.com/in/satyam-dubey-142598258/) & [Aditya Koranga](https://www.linkedin.com/in/aditya-koranga/)

**Published:** December 08, 2024

**Note:** This documentation is a replica of the README available at the [github/ngkore/cuPQC](https://github.com/ngkore/cuPQC). Please refer to the original repository for the most up-to-date information.

## Prerequisites

To utilize cuPQC, the following environment requirements must be met:

### System Requirements

- CPU: x86_64 architecture
- GPU: NVIDIA GPU with architecture 70, 75, 80, 86, 89, or 90
- Recommended Hardware: NVIDIA H100 (Development tested on RTX A4000)

### Software Requirements

- CUDA Toolkit: Version 12.4 or newer
- Host Compiler: C++17 support required (g++/gcc 11 or newer)
- Build Tools: CMake 3.20+ (optional)

### CUDA Installation (Ubuntu 22.04)

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.6.3/local_installers/cuda-repo-ubuntu2204-12-6-local_12.6.3-560.35.05-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-12-6-local_12.6.3-560.35.05-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-12-6-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-6

```

### Post-Installation Setup

Add the following to your `.bashrc` file to make changes permanent:

```bash
export PATH=/usr/local/cuda-12.6/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

```

Verify the installation:

```bash
nvcc --version

```

### Resources

cuPQC benchmarking video: [https://youtu.be/mdnXKroR1wo?si=yiTMTUy1MIzl20_7](https://youtu.be/mdnXKroR1wo?si=yiTMTUy1MIzl20_7)

## Build Instructions

1. Clone the repository and navigate to the examples directory:

```bash
cd cuPQC/examples/

```

2. Build using the provided Makefile:

```bash
make

```

Alternatively, build manually using `nvcc`:

```bash
nvcc -dlto -arch=native -std=c++17 -O3 -L../lib/ -lcupqc -lcuhash -o <binary_name> <file_name.cu> -I../include/ -I../include/cupqc

```

Note: Ensure the `-arch` flag matches your GPU's compute capability. If programs fail to execute, delete the binary and rebuild.

## Benchmarking

Tests were performed on an NVIDIA GH200.

### ML-KEM 512 Performance

Build and run the benchmark:

```bash
nvcc -dlto -arch=native -std=c++17 -O3 -L../lib/ -lcupqc -o v4_max_bench v4_max_bench.cu -I../include/ -I../include/cupqc
./v4_max_bench

```

Results:

- Keygen: ~20 million ops/sec
- Encapsulation: ~18.5 million ops/sec
- Decapsulation: ~18 million ops/sec

### Additional Benchmarks

- ML-KEM 768: `./mlkem768_bench`
- ML-KEM 1024: `./mlkem1024_bench`
- ML-DSA 44: `./bench_mldsa`
- ML-DSA 65: `./mldsa65_bench`
- ML-DSA 87: `./mldsa87_bench`
