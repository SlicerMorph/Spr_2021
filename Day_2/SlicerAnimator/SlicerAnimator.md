## SlicerAnimator 
`Animator` is a SlicerMorph specific module that helps to create and export keyframe based animation of 3D volumes in mp4 or GIF format. The animations are created by visualizing a volume and adjusting the rotation, ROI cropping, and volume rendering properties. A demo video of this module is also available [here](https://youtu.be/9GBekYcJR4E).

**Note** to do 3D spinning animations of 3D models, you can also use the default `Screen Capture` module. 

1. Load the MRIHead volume from the `Sample Data` module.

2. Open the `Volume Rendering` module. In the **Volume** field, make sure the volume MRHead is selected. Click the eyeball next to the **Volume** field to display the image. Under the Display Menu, adjust the Shift sliderbar to optimize 3D visibility.

2. Open the `Animator`  module. In the Animation Parameters dropdown menu, select the option to create a new animation. 

<img src="./images/animatorModule.png" width="800px"/>

3. Select the **Add Action** button and choose **CameraRotationAction** from the menu. A CameraRotation action will be added to the Action menu. The properties of the rotation can be adjusted using the **Edit** button. To preview the animation, select the play button. 

<img src="./images/addCamera.png" width="400px"/>

4. Select the **Add Action** button and choose **ROIAction** from the menu. Two ROI markups will be placed in the scene. The first ROI will be used to crop the region displayed at the start of the animation and the second will be used to crop the region displayed at the end of the animation. To adjust the placement of the ROIs, switch to the `Data` module and turn the visibility of the ROI markups on. Place the Start ROI around the whole head. Place the second ROI inside the brain. Return to the `Animator` module to preview the effect of this action. 

<img src="./images/selectROI.png" width="800px"/>

5. Select the **Add Action** button and choose **VolumePropertyAction**. The volume property action allows your animation to transition from one set of rendering properties to a second set. To use this effect, you will need to create three sets of volume properties. The first will control the rendering at the beginning of the animation, the second will control the rendering at the end of the animation, and the third is an arbitrary set of properties that will be used to store the volume's display properties as they are updated. To create the properties, click the **Edit** button next to the volume property action that you added to the Actions list. Note that three property sets are created  named **Start VolumeProperty**, **End VolumeProperty**, and **VolumeProperty**. 

6. Open the `Volume Rendering` module and expand the **Inputs** menu. In the Property drop-down menu, select **Start VolumeProperty**. The current rendering will be displayed at the start of the animation. Adjust the color mapping and opacity. 

<img src="./images/startProperty.png" width="800px"/>

You can repeat this process for the **End VolumeProperty**, but in this example we will use preset display properties for the final animation rendering. In the display menu, click the **Select a Preset** button and choose **MR-default**. This will display the volume as it will appear at the end of the aniamtion. Before leaving  the `Volume Rendering` module, return to the property menu and select **VolumeProperty**. It is critical that this is the final active property selected when leaving the `Volume Rendering` module.

<img src="./images/endProperty.png" width="400px"/>

7. Open the `Animator` module. Click the **Edit** button next to the Volume Property action in the Actions menu. Select the Start VolumeProperty as **Start VolumeProperty**, End VolumeProperty as **MR-default** and the Animated VolumeProperty as **VolumeProperty**. Click the **OK** button and preview the animation using the play button.

<img src="./images/volumeProperty.png" width="400px"/>

8. When you are done adjusting the animation parameters, Select an output file location in the Export menu and click the **Export** button to save your animation.