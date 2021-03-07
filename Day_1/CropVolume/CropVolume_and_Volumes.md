## Crop Volume and Volumes Module

We will review three Slicer modules that you will use frequently in this course:

1. `SampleData`: lets you download sample dataset bundled with Slicer. Extensions, such as SlicerMorph, can add their own sample datasets, in addition to the  sample datasets that is provided by Slicer. 
2. `Volumes`: which lets user view important information such as image spacing, orientation, data type etc about loaded 3D volume(s)
3. `CropVolume`: which lets user downsample, supersample or crop an existing volume with or without interpolation.

### Sample Data
1. Go to File->Download Sample Data and review the shown datasets. First 15 datasets are bundled with Slicer and second part is available only after you install the SlicerMorph extension. 

<img src="SampleData.png" width="600px"/>

2. Scroll down to SlicerMorph section, and click **sample_Skyscan_mCT_reconstruction.zip**. For the 1st time you use it, this file will be downloaded from internet to the Slicer's **Cache** folder. (If you don't know where that is, you can check it by going to Edit->Application Settings->Cache). Note that this is the same dataset we used in the `ImageStacks` tutorial. 

3. Unzip the contents of the archive to a convenient location. 

### Volumes

1. Follow the instructions from the `ImageStacks` tutorial to import the sample data from previous step. 

2. Go to `Volumes` module and review the **Volume Information** section to see where volume dimensions, image spacing and data type are reported (along with image geometry ).

<img src="Volumes1.png" width="600px" />

3. Next, go down to the **Display** section. This is where the lookup table (LUT) for volumes can be set. Greyscale is the default LUT for scalar volumes, but you can manually change this to more exotic LUTs. Contrasting LUTs such as <span style="color:green"> **Green**</span> or <span style="color:red"> **Red**</span> can be useful when you want to superimpose two volumes (e.g., a contrast-enhanced scan for soft-tissue, and a normal CT) and visualize the results.

<img src="Volumes2.png" width="800px" />

4. Window/Level slider lets you adjust the contrast of the image. This does not impact the voxel values, simply changes the contrast. You can also use the topbar icon to adjust the W/L. Expand the dropdown for W/L related functionality (e.g., choose a specific region for better contrast)

5. Review the **Interpolation** option and note that voxel boundaries are blurred when interpolation is enabled, which is the default setting in Slicer. While this results in smoother looking images in slice views, you may want to disable the interpolation during manual segmentations, particularly if you need to follow voxel boundaries for your task. **Note:** Interpolation option has no effect on the 3D rendering of the volume, it only impacts the slice views.

<img src="Volumes3.png" width="500px"/>

**IMPORTANT** If you have more than one volume loaded into your scene, the info given for the "Active Volume" may not be the volume have been looking at in slice views. Get into the habit of setting the slice view visibility of any volume by using the little eye icon in `Data` module. 

### Crop Volume
Regardless of whether you are only interested in a small subset of your volume, in Slicer the entire volume (and volumes if you are working with more than one dataset in the same scene) is kept in the memory (RAM). During segmentation, memory consumption may increase 10X (e.g. if you have 1GB dataset, at times you may need 10GB of available RAM). You can greatly reduce your memory footprint during segmentation, by creating a new volume that only contains the subset. Alternatively, if you have to work with the full extend of the data, but encounter memory issue during subsequent operations, you may want to downsample your volume.  `CropVolume` is the easiest way of achieving downsampling (or alternatively supersampling) your volumes. 

1. Continuing from the previous step, go to `CropVolume` and enter the settings shown below. If the ROI creates is too small, hit the **Fit ROI to Volume** button and hit Apply. Then choose to create a new volume that will contain the output of the operation. In my case I called it **left_side_damaged_reduced**, which will be reduced by 50% in each axis. Accordingly, the image spacing will doubled. Compare the reported input and output volume dimensions and spacing. This operation will result in 8 folds reduction in the data volume, at the expense of image detail.

<img src="CropVolume1.png" width="800px"/>

2. Now, repeat the procedure one more time, but this time set the limits of the ROI by modifying the small circular handles in slice views. I set my ROI to contain only the nasal region. Choose to create a new output volume (mine was called **Nasal_septum_and_turbinates**), and disable the interpolation. We do not need to interpolate the voxel intensity values, because we simply removing the data outside of our ROI and not doing anything else to the rest. Compare the resultant image dimensions and spacing of input and output volumes. Hit Apply. This procedure also reduces the data volume approximately 10 folds, but there is no reduction in the image quality. 

<img src = "CropVolume2.png">

More detail about [`CropVolume` can be found here.](https://www.slicer.org/wiki/Documentation/Nightly/Modules/CropVolume)

### IMPORTANT TECHNICAL NOTES

**Anisotropic data** If you are working with anisotropic voxels (i.e., different image spacing along different axes), you may want to enable **isotropic** option so that the resultant volume has isotropic spacing. Isotropic voxel alleviates some of the issues you may encounter with `Segment Editor` with anisotropic datasets. Images produced by medical CT scanners tend to have anisotropic spacing (usually Z axis is of lower resolution than X and Y). Most microCT scanner produce isotropic volumes. However, if you used the skipping slice option in the `ImageStacks` that will no longer be true after the import. 

**Do I need to downsample?** The answer to that depends on how much memory you have on your computer, how big is the volume you are working on and what you want to do with it. Consider a 3D volume that is 2000 slices, and 2000x2000 pixels in each slice, and voxel intensities are represented by 16 bit data (so 0-65535 range grayscale values). When the full volume is loaded without downsampling into Slicer, it will consume 16GB of memory (note that this is reported in the `ImageStacks` **Size** tab). If you want to visualize this volume you should have a GPU that has more than 16GB memory (since otherwise it won't fit into the texture memory). If you want to segment it you will need about 8-10X RAM than the volume (so you should have anywhere from 80-160GB of computer RAM). Also every semi-automated segmentation effect (e.g., threshold) will be applied to whole dataset, which means computations will take longer.  So you need a big, powerful computer. But if your use case is making high-quality rendering of only the skull, for example, then you can import the full volume at the **Preview** resolution via `ImageStacks`. Since this will reduce the data volume by factor of 64, you will have very managable dataset that you can explore, define the ROI that you would like to import at full resolution carefully and then decide what resolution you can import it by looking at the **output size** of the `ImageStacks`. 

**Oversampling:** Oversampling is opposite of downsampling. In some cases, you will want to oversample your dataset. For example, some thin structures can be only a voxel or two thick which would be problematic to segment or visualize. In such cases, oversampling the data is helpful. Oversampling will not increase the resolution (aka detail) in your data. It will simply subdivide the voxels. Oversampling by 2, will split a single cubic voxel into 8 smaller cubic voxels, so that some mathematical operations such as smoothing or dilation/erosion can work more effectively. However, this will increase the memory usage considerably and should be better applied to subsets of data. You will see more of this in `Segmentation` tutorial. 


