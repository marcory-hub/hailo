# hailo

## Training a Custom YOLOv8 Model for Raspberry Pi 5 with Hailo-8L AI kit

#### How to 
- Train a YOLOv8 model from the Hailo Model Zoo with your custom dataset in Google Colab 
- Optimize and compile it with a docker installation of the Hailo Software Suite on Google Cloud Platform (GCP) (pay-as-you-go GPU instances)
- Deploy you custom YOLOv8 model on a Raspberry Pi 5 equipped with the Hailo-8L AI kit.

#### Benefits
- No local x86 Unix training computer needed.
- Free Training: Utilize free GPU resources in Colab for efficient training.
- Customizable Model: Train a model to detect objects specific to your needs.
- Hailo-8L Acceleration: Achieve faster and efficient inference on your Raspberry Pi 5.

#### Prerequisites
- Familiarity with Python and basic object detection concepts.
- A Google account (for Colab and GCP).
- Raspberry Pi 5 with Hailo-8L AI kit installed. For instructions see [How to Set Up Raspberry Pi 5 and Hailo-8L](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
- Utilize the basic detection pipeline. For instructions see [Hailo RPi5 Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#installation)
- (Optional) paid subscription to GCP if you want to optimize the model. 

# Steps
1. Prepare Dataset
2. Make a YOLOv8 Model in Colab
3. (Optional) Create a Virtual Machine in Google Cloud Platform (Free)
4. (Optional) Docker Install Hailo Software Suite (Free)
5. Virtual Machine in GCP with T4 GPU in Docker with the Hailo Software Suite
6. Deploy Model on Raspberry Pi 5

## 1. Prepare Dataset

Gather images containing the objects you want to detect.
Annotate these images using a tool like [CVAT](https://www.cvat.ai/) in YOLO1.1 format (bounding boxes and class labels). Or use the [hornet3000+](https://www.kaggle.com/datasets/marcoryvandijk/vespa-velutina-v-crabro-vespulina-vulgaris) dataset, more information on the YOLOs8n model on a Raspberry Pi4 8GB can be found [here](https://github.com/vespCV/hornet3000).

## 2. [Make YOLOv8s Model on Colab](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb)

Follow this notebook [hailo_YOLOv8s Google Colab notebook](https://github.com/marcory-hub/hailo/blob/main/hailo_YOLOv8s.ipynb) to train a YOLOv8 model using your custom dataset in Colab. The training process will generate a **best.onnx** file, which represents your trained model. 

Do not forget to download the model before you stop the notebook.

## 3. [Create a Virtual Machine in Google Cloud Platform (no GPU)](https://github.com/marcory-hub/hailo/blob/main/create-and-connect-gcp-vm-instance-w-local-terminal.md)
_If you're using pay-as-you-go GPU instances, you can skip this section_

While Colab offers free GPU resources, the virtual machine (VM) is needed to do the docker install of the Hailo Software Suite. A GPU is not available in the Free Trial. This is usefull to get familiar with the VM, but does not optimize you model in step 6.

## 4. [Docker Install of the Hailo Software Suite](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md)
_If you're using pay-as-you-go GPU instances, you can skip this section_

The suite installation as a Docker file is needed for step 6 optimizing and conversion.

## 5. [Virtual Machine in GCP with T4 GPU in Docker with the Hailo Software Suite]()
_If you're NOT using pay-as-you-go GPU instances, you can skip this section_

Optimize and compile a yolo model with a Dockerized Hailo Software Suite on Google Cloud Platform (GCP) using T4 GPU instances.

## 6. [Compile the Model using Hailo Model Zoo](https://github.com/marcory-hub/hailo/blob/main/compile-the-model-using-hailo-model-zoo.md)
Use Hailo Model Zoo within the Docker container to convert your model to a Hailo-optimized format (yolov8s.hef).

## 6. [Deploy Model on Raspberry Pi 5](https://github.com/marcory-hub/hailo/blob/main/deploy-model-on-raspberry-pi-5-ai-kit.md)

Transfer the Hailo Executable File **yolov8s.hef** file to your Raspberry Pi 5. This will involve setting up the Hailo runtime environment and integrating your model into your application.
