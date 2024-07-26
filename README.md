# Sentiment Sphere

## Overview

Sentiment Sphere is a project that involves training and deploying a serverless Sentiment Analysis API on Google Cloud Platform (GCP). The project leverages BERT (Bidirectional Encoder Representations from Transformers), TensorFlow, and FastAPI to create a scalable and efficient sentiment analysis solution. We use various GCP services, including Google AI Platform Training, Google Storage, Cloud Build, Cloud Container Registry, and Cloud Run, to manage and deploy the model and its prediction service.

## Project Structure

- **`training/`**: Contains the code and configuration files for training the sentiment analysis model.
- **`prediction/`**: Contains the code and configuration files for deploying the prediction service.

## Training the Sentiment Model

To train the sentiment model, follow these steps:

1. **Configure Training Environment**

   - Edit the `training/cloudbuild.yaml` file to match your GCP environment settings. Ensure all necessary configurations are set for your environment.

2. **Build the Training Docker Image**

   - Build the Docker image used for training by running the following command:

     ```bash
     gcloud builds submit --config training/cloudbuild.yaml
     ```

3. **Submit the Training Job**

   - Set the environment variables and submit the training job to AI Platform:

     ```bash
     export JOB_NAME=bert_$(date +%Y%m%d_%H%M%S)
     export IMAGE_URI=gcr.io/[YOUR_PROJECT_ID]/sentiment-training:latest
     export REGION=us-west1

     gcloud config set project [YOUR_PROJECT_ID]

     gcloud ai-platform jobs submit training $JOB_NAME \
       --region $REGION \
       --master-image-uri $IMAGE_URI \
       --scale-tier=BASIC_GPU
     ```

   - Replace `[YOUR_PROJECT_ID]` with your actual GCP project ID.

## Deploying the Prediction Service

To deploy the prediction service, follow these steps:

1. **Configure Deployment Environment**

   - Edit the `prediction/cloudbuild.yaml` file according to your GCP environment settings.

2. **Build and Deploy the Prediction Service**

   - Build and deploy the prediction service to Cloud Run using Cloud Build:

     ```bash
     gcloud builds submit --config prediction/cloudbuild.yaml
     ```
     
## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
