## Train Me Up

Our main objective is to use Unreal Engine 4 based DataSynthesizer by NVIDIA to generate synthetic labelled data for a Kinova Arm. Followed by training a supervised neural network model to predict the joint poses of a kinova arm given its RGB image.

### Synthetic Data Generation for Robot Pose Estimation w.r.t. Camera

- We created a Rigged skeleton of Kinova Arm (Jaco 2) in Blender. 
- This was then imported in UE4 where a couple of animation sequences were generated. 
- Following this we created augmented data using NVIDIA data synthesizer. The original code base lacks the capability of exporting the joint positions in the augmented RGB image. We have enabled this capability by twiking their C# code.

### Kinova Arm Pose Estimation

Having a neural network capable of predicting the joint poses from a RGB image equips the researcher to setup a camera (e.g.,on a tripod) and detect the robot pose in an independent manner without worrying about camera disturbances or calibrations. This would avoid enormous efforts that are needed to calibrate the camera each time it’s setting is changes.
