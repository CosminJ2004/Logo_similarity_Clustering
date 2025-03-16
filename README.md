# README: Company Logo Fetching &  Image Clustering 

## Solution Explanation

### Problem Overview

I indetified two problems that needed to be solved so this project has two main components:

1. **Logo Fetching**: The goal was to create a system that can automatically download and store company logos from the internet, specifically using the Clearbit API for logos based on domain names.
   
2. **Image Clustering**: The second part of the project focuses on clustering logos based on their visual similarity using deep learning models such as ResNet50, VGG16, or MobileNetV2, and KMeans for unsupervised learning.

---

### **Logo Fetching Approach**

To fetch company logos based on their domain names, I tried setting up a **Google Cloud** Service API and search each company logo from the domain provided. This approach had poor results (aprox 200 logos) so I decided to utilize the **Clearbit API**. The approach for fetching logos is as follows:

1. **API Request**: For each domain name in the list, a request is made to `https://logo.clearbit.com/{domain}` to fetch the logo.
2. **Error Handling**: If the logo is not available for a specific domain, I print an appropriate error message and skip that domain.
3. **Storage**: When the logo is successfully fetched, it is saved locally in the `logos/` folder. This allows for easy access and organization of logos later.

By using the Clearbit API, I could ensure that I fetched high-quality logos directly from a trusted source, minimizing the risk of errors and inconsistencies(aprox 2900 logos).

---

### **Image Clustering Approach**

For clustering, the goal was to group visually similar logos based on extracted features, leveraging the power of pre-trained deep learning models.
It was clear to me from the start that i needed to use a CNN but I had to test which one was best in this case. 
My first choice was the VGG16, then the ResNet50 but both turned out to be too demanding for my resources so i used MobileNetV2, not the best model but it works well with simple logos, especially ones that have similar backgrounds or fonts.
The solution works as follows:

1. **Pretrained Models**: I utilized three different deep learning models: **ResNet50**, **VGG16**, and **MobileNetV2**. These models are pre-trained on ImageNet, meaning they are capable of extracting meaningful features from images, including logos. 
   - **Why MobileNetV2?** While ResNet50 and VGG16 are both strong models, MobileNetV2 is more lightweight and resource-efficient, which made it an ideal choice for limited hardware resources.

2. **Feature Extraction**: Using the chosen model, I extracted features from each logo image. This was done by removing the top layer of the models and using the output from the last convolutional layers as the image features.
   
3. **Dimensionality Reduction**: After extracting the features, I applied **PCA (Principal Component Analysis)** for dimensionality reduction. PCA helps to reduce the number of features and improve the clustering performance, especially whith images.

4. **Clustering**: Once the features were extracted, I applied the **KMeans algorithm** to cluster the logos. The number of clusters is configurable, and each logo is assigned a label corresponding to its cluster. Logos within the same cluster are visually similar according to the model’s learned features.

5. **Output**: After clustering, the logos are stored into separate folders, where each folder corresponds to a different cluster. This results in a well-organized structure where similar logos are grouped together for further analysis or visualization.

---

### **Why This Approach?**

I chose to use pre-trained models for feature extraction due to their ability to generalize well across different types of images, including logos. Training a custom model from scratch on logo data would have been computationally expensive and time-consuming, so leveraging existing models like ResNet50, VGG16, and MobileNetV2 made sense.

For clustering, I opted for KMeans because it's a simple and widely-used unsupervised algorithm. It allows me to group logos based on similarity without needing labeled data. By using PCA, I was able to improve the quality of clustering and handle the high-dimensional nature of the feature space efficiently.

I found that 40 is an optimum number of clusters, anything more would just place each logo firm in a different cluster, and less would have less variance.

---

## Output

After running the program, the output will be a series of folders inside an `output/` directory, where each folder represents a different cluster. The logos in each folder are visually similar based on the features extracted from the pre-trained models.

