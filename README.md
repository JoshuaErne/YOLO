# YOLO

YOLO (You Only Look Once) is a popular object detection algorithm that uses a single neural network to detect objects within an image. YOLO divides an input image into a grid of cells and predicts bounding boxes and class probabilities for each cell. The algorithm is able to detect multiple objects within a single image in real-time, making it well-suited for applications that require fast inference times.

One of the advantages of YOLO is its simplicity and speed compared to other object detection algorithms, such as Faster R-CNN. YOLO is also able to generalize well to new object categories and is relatively robust to occlusion and clutter.

However, YOLO can struggle with detecting small objects and objects with low contrast, and its performance may be lower than other algorithms on datasets with a large number of object categories. Nonetheless, YOLO has been widely adopted in various applications, including autonomous driving, robotics, and surveillance.

In this project, I have implemented YOLO-v1 using 10K street scene images to detect objects belonging to the vehicle, people and traffic light classes. The input data include the images along with the labels as training data. The image dimension is 128X128X3 and the labels consist of semantic class together with the bounding boxes corresponding to each object in the image.

# Network Architecture
![image](https://user-images.githubusercontent.com/42107613/204451561-c44b8f5b-adb1-4a6b-98ab-191b527adf1b.png)

# Training and Loss Function
![image](https://user-images.githubusercontent.com/42107613/204451937-0c675e2b-a4d9-4a0a-b906-1cb019730b99.png)

# Results

<img src=imgs/output.png> <p></p>
<img src=imgs/allboundingboxes.png> <p></p>
<img src=imgs/postlowconfsupression.png> <p></p>

# Observation
Given the substantially lower training loss compared to the validation loss and the significantly bigger training mAP compared to the validation map, the main issue is the dataset is too small to work on and splitting the small dataset leads to overfitting. We tried  altering the network architecture, drastically increased the number of epochs (took for ever with colab) and we also changed the optimizer to perform with different learning rates and weight decays but it didnt work. 

Another issue was an imbalance in class labels. It would have been better if we could have maybe seperated out the model to learn each class individually (just a thought). For example, few images had very few traffic lights and more cars or vice versa. By viewing the 8x8x8 output and concentrating on the different depths (objectness, probability of classes etc) we could see that the model does improve over time and does improve predictions over a period of time but the number of false positives always kept fluctuating (example: where the objectness should be 0 with respect to the target, the predictions are close to 1 in very few cases).

The IOU kept penalizing the ability to predict for the pedestrian and traffic light classes while computing the YOLO Loss. So getting rid of them in the third and fourth term of the Yolo loss significantly helped improve the performance. Also, we had to define various reconstruction cases for each sort of data we were dealing with and that was pretty annoying.

