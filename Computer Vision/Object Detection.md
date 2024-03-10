![[Pasted image 20240226221543.png]]
![[Pasted image 20240226221556.png]]
![[Pasted image 20240226221607.png]]
![[Pasted image 20240226221618.png]]
![[Pasted image 20240226221655.png]]
![[Pasted image 20240226221706.png]]
In object detection, a feature map plays a crucial role because it contains the distilled information of the input image, highlighting important patterns while discarding irrelevant details. Here's how feature maps are useful in object detection:
1. **Hierarchical Feature Learning**: Convolutional Neural Networks (CNNs), like AlexNet, learn a hierarchy of features. Early layers may detect simple patterns like edges and textures, while deeper layers can detect more complex features like parts of objects or even entire objects. Feature maps at different levels represent these features at different scales and complexities.
2. **Localization**: As mentioned in the slide from the image you provided, each unit in the feature map corresponds to a region in the original image. This correspondence allows the network to not only recognize the presence of an object but also infer its location based on the activations within the feature map. For example, if a particular unit in the feature map is highly activated, it suggests that the corresponding region in the input image contains a feature of interest.
3. **Translation Invariance**: Due to the pooling layers and the nature of convolution operations, CNNs are somewhat invariant to the translation of objects in the input space. This means that if an object moves around in the image, it will still activate the same features on the map, aiding in detection regardless of the exact position.
4. **Object Scale**: Feature maps at different stages of the CNN can detect objects of different sizes. Earlier maps might be better at detecting smaller objects, as they're closer to the original input size, while later maps can detect larger objects due to the receptive field size increasing with each layer.
5. **Channel Depth**: Each channel in a feature map can be thought of as a detector for a different type of feature. In object detection, different channels may activate strongly in response to different parts of an object or different types of objects altogether. Analyzing these channels can help in detecting various attributes of objects.
6. **Integration with Detection Frameworks**: In modern object detection frameworks like R-CNN, YOLO, or SSD, feature maps are used to generate region proposals or bounding boxes. These methods often use additional layers on top of the base CNN to predict object categories and bounding box coordinates directly from the feature maps.
7. **Semantic Segmentation**: In tasks that require a pixel-wise classification, such as semantic segmentation, feature maps can be upsampled back to the size of the original image to predict the class for each pixel, which is essential for detailed object detection and delineation in an image.
![[Pasted image 20240226222216.png]]
![[Pasted image 20240226222228.png]]
- We combine feature map and region proposal networks via RoI pooling to get classifier output
![[Pasted image 20240226222409.png]]
![[Pasted image 20240226222511.png]]
![[Pasted image 20240226222520.png]]
- **Anchors**: For each position of the sliding window, the network generates multiple region proposals, called "anchors." These anchors are predefined and are of various sizes and aspect ratios. In the example provided, there are 3 scales (128², 256², 512² pixels) and 3 aspect ratios (1:1, 1:2, 2:1), which combine to make $k=3*3=9$ different anchors at each sliding window position.
- **Convolutional Features**: The convolutional feature map, which is input to the RPN, provides context about objects at different scales. During training, the network learns to recognize patterns that are indicative of objects at different scales and aspect ratios. For instance, the presence of certain features might signal the likely scale and aspect ratio of an object at a certain location.
- **Prediction**: For each anchor, the RPN predicts two sets of values:
	- **Objectness Scores**: It outputs a score that estimates how likely the anchor is to contain an object versus being part of the background.
	- **Bounding Box Refinements**: It also predicts four coordinates that refine the anchor box to more tightly fit the object. This is typically a regression task, where the network adjusts the size and position of the anchor box.
- **Intermediate Layer**: There’s usually an intermediate layer (e.g., 256-d) that connects the feature map to the cls (classification) and reg (regression) layers. This layer processes the features to make them suitable for predicting the presence of objects and their locations.
- **Integration with Feature Map**: The anchors are defined relative to the original image size, but they are applied to the feature map. Since the feature map is a downscaled version of the input image, each unit of the feature map covers a larger portion of the input image. The network learns to adjust predictions based on the receptive field of each unit in the feature map.
![[Pasted image 20240226223305.png]]
![[Pasted image 20240226223328.png]]
![[Pasted image 20240226223352.png]]
- Classification: categorising image into predefined classes/categories
- Localisation: involves identifying the location and extent of objects within bounding boxes or segmentation masks.
![[Pasted image 20240226223607.png]]
![[Pasted image 20240226223630.png]]
![[Pasted image 20240226223722.png]]
![[Pasted image 20240226223816.png]]
![[Pasted image 20240226223840.png]]
![[Pasted image 20240226223859.png]]
![[Pasted image 20240226223911.png]]
![[Pasted image 20240226224116.png]]
![[Pasted image 20240226224126.png]]
![[Pasted image 20240226224201.png]]
![[Pasted image 20240226224224.png]]
![[Pasted image 20240226224240.png]]
