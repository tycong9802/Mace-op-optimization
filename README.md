# MACE upload final
final project for mace.

The original Mace GitHub repository link is: https://github.com/XiaoMi/mace

This repository contains the final project for Mace, which includes the implementation of two deep neural networks (DNN) models, ShuffleNet v2+ small and RegNet (200M), and instructions on how to run the code successfully.

To use the RegNet DNN model, users can download and open the RegNetUp file, and follow the provided instructions in the final report to run the code. For the ShuffleNet DNN model, users can download and open the ShuffleNetUp file, and similarly follow the instructions provided in the final report to run the code.

To utilize the DNN models, users must also download the corresponding .yml file and model file. It is important for users to change the path in the .yml file based on the path of the model file and their own sha256.

I ensure the code can run successfully on both Linux and MacOs with M1 chip.

Overall, this project showcases successful implementation of these two popular DNN models, providing a valuable resource for researchers and practitioners in the field.

## Set up

### Environment requirement
MACE requires the following dependencies:

|  Software   | Installation command  |
|  ----  | ----  |
| Python  | 2.7 or 3.xx |
| CMake | Linux: apt-get install cmake /// Mac:brew install cmake	 |
| Jinja2 | 	pip install jinja2==2.10 |
| PyYaml | pip install pyyaml==3.12 |
| sh | pip install sh==1.12.14 |
| Numpy | pip install numpy==1.14.0 |
| six | pip install six==1.11.0 |

## Basic usage for CMake users
First of all, make sure the environment has been set up correctly already.

### Clear Workspace
Before you do anything, clear the workspace used by build and test process.
```
  tools/clear_workspace.sh [--expunge]
```
### Build Engine
Please make sure you have CMake installed.
```
RUNTIME=GPU QUANTIZE=OFF bash tools/cmake/cmake-build-armeabi-v7a.sh
```
which generate libraries in `build/cmake-build/armeabi-v7a`, you can use either static libraries or the `libmace.so` shared library.

You can also build for other target abis: `arm64-v8a`, `arm-linux-gnueabihf`, `aarch64-linux-gnu`, `host`; and runtime: `GPU`, `HEXAGON`, `HTA`, `APU`.

### Model Conversion
When you have prepared your model, the first thing to do is write a model config in `YAML` format. For example:
```
library_name: mobilenet-v2_1_0
target_abis: [armeabi-v7a, arm64-v8a]
model_graph_format: file
model_data_format: file
models:
  mobilenet:
    platform: onnx
    model_file_path: /Users/path/to/model/regnet_opt.onnx 
    model_sha256_checksum: 2defjieojoejabce3020r03107d3c47ce4461e390fa4
    subgraphs:
      - input_tensors: input
        output_tensors: output
        input_shapes: 1,224,224,3
        output_shapes: 1,1000
    runtime: cpu+gpu
    limit_opencl_kernel_time: 0
    nnlib_graph_mode: 0
    obfuscate: 0
```
The following steps generate output to `build` directory which is the default build and test workspace. Suppose you have the model config in `../mace-models/mobilenet-v1/mobilenet-v1.yml`. Then run
```
python tools/python/convert.py --config ../mace-models/mobilenet-v1/mobilenet-v1.yml
```
which generate 4 files in `build/mobilenet_v1/model/`
```
├── mobilenet_v1.pb                (model file)
├── mobilenet_v1.data              (param file)
├── mobilenet_v1_index.html        (visualization page, you can open it in browser)
└── mobilenet_v1.pb_txt            (model text file, which can be for debug use)
```
MACE also supports other platform: `caffe`, `onnx`. Beyond `GPU`, users can specify `cpu`, `dsp` to run on other target devices.

### Model Test and Benchmark
We provide simple tools to test and benchmark your model.

After model is converted, simply run

```
python tools/python/run_model.py --config ../mace-models/mobilenet-v1/mobilenet-v1.yml --validate
```
Or benchmark the model
```
python tools/python/run_model.py --config ../mace-models/mobilenet-v1/mobilenet-v1.yml --benchmark
```
It will test your model on the device configured in the model config (`runtime`). You can also test on other device by specify `--runtime=cpu (dsp/hta/apu)` when you run test if you previously build engine for the device. The log will be shown if `--vlog_level=2` is specified.



