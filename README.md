# Oobabooga text-webui and Automatic1111 Docker-files


# Project Setup Guide

This guide will help you set up the **stable-diffusion-webui** and **text-generation-webui** projects on your computer. Run both and connect them together. Make sure you follow these steps to get everything up and running smoothly.

## Prerequisites

Before starting, make sure you have the following prerequisites installed:

- Docker: [Install Docker](https://docs.docker.com/get-docker/)
- [NVIDIA GPU Driver](https://www.nvidia.com/download/index.aspx) (if you plan to use GPU acceleration)

## Download Project Files

1. Download the following project zip files to your preferred location on your computer:

    - [stable-diffusion-webui_v1.6.zip](https://github.com/HFarkhari/Oobabooga-text-webui_and_Automatic1111_Docker-files/blob/main/stable-diffusion-webui_v1.6.zip) (Stable Diffusion Web UI)
    - [text-generation-webui_v1.5.zip](https://github.com/HFarkhari/Oobabooga-text-webui_and_Automatic1111_Docker-files/blob/main/text-generation-webui_v1.5.zip) (Text Generation Web UI)

2. Extract the contents of these zip files. Ensure that the resulting folder names and paths match the following:

    - **Stable Diffusion Web UI**:
        - Folder Path: `~/Desktop/stable-diffusion-webui`
    - **Text Generation Web UI**:
        - Folder Path: `~/Desktop/text-generation-webui`

## Docker Image Setup

### For Stable Diffusion (Automatic1111)

1. Pull the Docker image for Stable Diffusion:

    ```shell
    docker pull hfarkhari/automatic_1111:v1.6_gradio_3.43.2_user_p7860_sadtalker
    ```

2. Prepare the Stable Diffusion environment by running the following command to create the necessary directories on your local machine:

    ```shell
    docker run --gpus all -it --rm -v $(realpath ~/Desktop/stable-diffusion-webui):/workspace hfarkhari/automatic_1111:v1.6_gradio_3.43.2_user_p7860_sadtalker python launch.py
    ```

3. Replace the `ddpm.py` file located at:

    ```
    stable-diffusion-webui/repositories/stable-diffusion-stability-ai/ldm/models/diffusion/ddpm.py
    ```

4. Run the Automatic1111 Docker image in normal mode. The first run will download additional dependencies:

    ```shell
    docker run --gpus all --net=host --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -p 7860:7860 -it --rm --shm-size=16g -v $(realpath ~/Desktop/stable-diffusion-webui):/workspace hfarkhari/automatic_1111:v1.6_gradio_3.43.2_user_p7860_sadtalker
    ```

5. Install SadTalker from the extension tab, apply the changes, close the Docker image, and then re-run the Docker image as in step 4. You're done!

### For Text Generation Webui (Oobabooga)

1. Run the following command to set up the Text Generation environment:

    ```shell
    docker run --gpus all --net=host --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -p 7777:7777 -it --rm -v $(realpath ~/Desktop/text-generation-webui):/workspace hfarkhari/text_web_ui:v1.5_gradio_3.43.2_user_p7777
    ```

## Accessing the Projects

- **Stable Diffusion (Automatic1111)** can be accessed at `http://127.0.0.1:7860` in your web browser.
- **Text Generation (Text-webui)** can be accessed at `http://127.0.0.1:7777` in your web browser.

## Additional Models and Files

You may need to download LLM models for Text-webui and Stable Diffusion models for Automatic1111 and place them in the appropriate subdirectories. Any additional files required will be downloaded to their respective folders in `~/Desktop/stable-diffusion-webui` or `~/Desktop/text-generation-webui` and stored in the `.cache` subfolder.

For more information on how to download Docker files and run them, check the following links:

- [Docker Text-webui Image](https://hub.docker.com/r/hfarkhari/text_web_ui)
- [Docker Automatic_1111 Image](https://hub.docker.com/r/hfarkhari/automatic_1111)

**Important**: Use the `--net=host` command when you run your Docker images to ensure that these two containers are in the same network and can find each other and communicate with `--api` commands. In this case, `-p 7777:7777` and `-p 7860:7860` commands will be ignored and can be removed. Make sure that ports 7777 and 7860 are free before running Docker files or use other custom ports for each Docker image.
