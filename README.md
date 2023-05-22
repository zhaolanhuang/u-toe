U-TOE: Universal TinyML On-board Evaluation Toolkit for Low-Power AIoT
=====================

# Get Source Code

It is important to clone the submodules along, with `--recursive` option.

```
git clone --recursive https://github.com/zhaolanhuang/U-TOE.git
```

Or open the git shell after clone, and type the following command.

```
git submodule init
git submodule update
```

# Prequisites
This toolkit is tested under:
- Linux Mint 20.6
- Python 3.10.1

## Install TVM from Source
First, Follow the instruction from [official docs](https://tvm.apache.org/docs/install/from_source.html).

It is recommended to switch on the following options in `config.cmake`:
```
USE_GRAPH_EXECUTOR 
USE_PROFILER 
USE_MICRO
USE_LLVM
```
If you're using CUDA, then switch `USE_CUDA` on.
For PyTorch users, it is recommended to set `(USE_LLVM "/path/to/llvm-config --link-static")` and `set(HIDE_PRIVATE_SYMBOLS ON)`.

After built up the TVM libraries, set the environment variable to tell python where to find the packages:
```
export TVM_HOME=/path/to/tvm
export PYTHONPATH=$TVM_HOME/python:${PYTHONPATH}
```

## Install RIOT Toolchains
Please refer to the doc [Getting started - Compiling RIOT](https://doc.riot-os.org/getting-started.html#compiling-riot)

For RISC-V toolchain, please enable __multilib__ support.

## Install Python Packages
Use the command below to install the dependencies:
```
pip install -r requirements.txt
```

# Usage
Befor running the evaluation, please get your model files ready from ML frameworks (TFLite, PyTorch etc.). You can find some model file examples in `model_zoo` folder.

```
> python u-toe.py -h

usage: u-toe.py [-h] [--per-model | --per-ops] [--board BOARD] [--use-iotlab] [--iotlab-node IOTLAB_NODE] [--random-seed RANDOM_SEED] [--trials-num TRIALS_NUM] model_file

positional arguments:
  model_file            path to machine leearning model file.

options:
  -h, --help            show this help message and exit
  --per-model           Per-Model Evaluation (default).
  --per-ops             Per-Operator Evaluation.
  --board BOARD         IoT board name
  --use-iotlab          use remote board in FIT IoT-LAB.
  --iotlab-node IOTLAB_NODE
                        remote node url. It will start an new experiment if this field is empty.
  --random-seed RANDOM_SEED
                        default: 42
  --trials-num TRIALS_NUM
                        defalut: 10
```

## Per-Model Evaluation
Example:

```
python u-toe.py --per-model --board stm32f746g-disco ./model_zoo/mnist_0.983_quantized.tflite
```
## Per-Operator Evaluation
Coming soon...