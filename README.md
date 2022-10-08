## Train Me Up: Kinova Arm Pose Estimation

Having a neural network capable of predicting the joint poses from a RGB image equips to setup a camera (e.g.,on a tripod) and detect the robot pose in an independent manner without worrying about camera disturbances or calibrations. This would avoid enormous efforts that are needed to calibrate the camera each time it’s setting is changes. Our main objective is to use Unreal Engine 4 based [NVIDIA NDDS](https://github.com/NVIDIA/Dataset_Synthesizer) which is a deep learning data synthesizer to generate synthetic labelled data for a Kinova Arm (Jaco 2). Followed by we train a supervised neural network model to predict the joint poses of a kinova arm given its RGB image. This work is inspired by [Lee et al., 2018](https://arxiv.org/pdf/1911.09231.pdf)'s work on Camera-to-Robot Pose Estimation from a Single Image.

## Synthetic Data Generation
We genenerated augmented data using NVIDIA data synthesizer in UE4.

### Prepare them Static Mesh of Kinova Arm 

- The kinova arm CAD model is available in .step format. 
- The .step file has large number of static meshes. 
- The static meshes needs to be combined such that we have 1 static mesh for each major part of the robotic assembly.
- Visual Dataprep and Datasmith plugin have been used for this purpose.

### Prepare the skeletal mesh of Kinova Arm

- Unreal Engine does not provide with modelling option. In order to define the bone structure for the robotic arm, we have used Blender.
- The resulting model was exported as a .fbx file. It contained the skeletal mesh of the robotic arm.
<p align="center">
<img width="211" alt="image" src="https://user-images.githubusercontent.com/76990931/194726165-350d246a-84de-4e56-a774-da34e58770da.png">
</p>

### Generate Animation Sequence in UE4

- Control Rig plugin is used to define the rig control in UE4
- The resulting control rig asset is used to create the animation sequence in UE4. 
- Finally the animation sequence asset is exported as a .fbx file.

<p align="center">
<img width="379" alt="image" src="https://user-images.githubusercontent.com/76990931/194726675-ee8778fa-6626-4c07-88ba-047515454e54.png">
</p>

You can see the sequence generated [here](https://drive.google.com/file/d/1WV0EGqOyeJcTbL4vSPbNUhj1DNRRfVZe/view).

### Integrate NDDS Project

- NDDS project is compatible only with UE4 22.3 and 22.1. 
- Hence, NDDS project is loaded using UE 4.22.3. Animation sequence .fbx model is then imported into the content folder.
- Following this a level is created which has a camera asset, the robotic arm animation and the supporting assets.

<p align="center">
<img width="234" alt="image" src="https://user-images.githubusercontent.com/76990931/194726764-411311d9-4f01-41f3-b3f3-fc21b5bd3de1.png">
</p>
  
An additional capability to export the Socket Data i.e. the  pose for each joint in the environment has been added to C# code base. Detailed steps for integrating this in mentioned in [Documentation](https://github.com/JBVAkshaya/KinovaArmUE4/blob/main/Documentation.pdf)
  
### Generate Data
  
- When we play the level we export,
  - A .json file containing the camera intrinsic parameters
  - Image corresponding to each frame as captured by the camera
  
<img width="490" alt="image" src="https://user-images.githubusercontent.com/76990931/194726956-5b053138-c8e1-445d-97f6-02f709c7d36f.png"> <img width="490" alt="image" src="https://user-images.githubusercontent.com/76990931/194727017-3095fbc9-34c1-47af-9ebb-0fc16acfc847.png">



### Initial Training Results

We implemented a simple Convolutional Neural Network that takes the augmented RGB images generated using UE4 as input and predicts all the joint coordinates for Kinova Arm (Jaco 2)
  
<p align="center">
<img width="745" alt="image" src="https://user-images.githubusercontent.com/76990931/194727191-1dda079a-1b4e-42b5-bd85-018c12ccc617.png">
</p>
 

In its current form we trained a very basic neural network with extremely limited data as our motivation was to develop this entire pipeline. This implementation can be easily extended to train more complex neural networks capable of predicting joint coordinated with much higher accuracies.
