# maceuploadfinal
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
| CMake | Linux:apt-get install cmake Mac:brew install cmake	 |
| Jinja2 | 	pip install jinja2==2.10 |
| PyYaml | pip install pyyaml==3.12 |
| sh | pip install sh==1.12.14 |
| Numpy | pip install numpy==1.14.0 |
| six | pip install six==1.11.0 |

