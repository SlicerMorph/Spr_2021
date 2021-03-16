<p align="center">
<img src="images/option5.png" alt="ALPACA logo" width='500' height='200' >
</p>

## Automated landmarking through pointcloud alignment and correspondence analysis

`ALPACA` provides fast landmark transfer from a 3D model and its associated landmark set to target 3D model(s) through pointcloud alignment and deformable mesh registration. Unlike the Slicermorph's semi-landmark methods, it does not require presence of fixed landmarks. Optimal set of parameters that gives the best correspondence can be investigated (and outcome can be visualized) in single alignment mode, and then applied to a number of 3D models in batch mode. Invoked first time, `ALPACA` needs your permission to download `open3D` library. Depending on the internet speed, download may take sometime but it is a one-time event.

## Walk-through

Let's start our walk-through by finding `ALPACA` within Slicer. Please find the navigation drop-down menu and then click:
  * __SlicerMorph > Geometric Morphometrics > ALPACA__ 
<br />
<p align="center">
<img src="images/scs01a.png" width='600' height='500'>
</p>
<br />
:pencil2:  If this is the first time you are opening `ALPACA`, it will ask you if you are ok with installing `open3d`. If you are using a Windows machine, the installation process can take a few minutes. 

Otherwise, you should observe the following screen:

<br />
<p align="center">
<img src="images/scs02.png" width='1280' height='600'>
</p>
<br />

A closer examination of the module's main menu reveals that there are two main tabs in `ALPACA` : a `Single aligment` and a `Batch processing` one.


## Single alignment 

* __Single alignment options__

  * __Source mesh__: Under the `Source mesh`, the user is expected to select the path to the `*.ply` mesh file to be used as a template
  
  * __Source landmarks__: Under the `Source landmarks`, the user is expected to select the path to the `*.fcsv` file containing the landmarks to be transferred to the target mesh.
  
  * __Target mesh__: Under the `Target mesh`, the user is expected to select the path to the `*.ply` mesh file to be used as a target (i.e., the specimen we are interested in predicting landmark positions for).
  
  * __Skip scaling__: Optional argument that determines whether the source mesh should be scaled to match the size of the target mesh.
  * __Skip projection__: Optional argument that determines whether the final landmark predictions should be projected to the surface of target mesh.

<br />
<p align="center">
<img src="images/scs02a.png" width='500  ' height='700'>
</p>
<br />

* Note that the `Single aligment` tab also contains an `Advanced parameter settings` menu that can be expanded. This is generally not recommended for novice users, but hyperparameter tuning can significantly improve the end result. In general, the `Point Density Adjustment` and the `Deformable registration` parameters are the most likely ones to improve the quality of the registration. The `Point Density Adjustment` parameter regulates the voxel size spacing used when resampling the original meshes.In general, the goal should be to find a voxel size that results in a total of `5000 - 6000` points per mesh. This is essential for optimal performance and will be discussed further below. Parameter `Alpha` is a regularization parameter that tends to affect the length of the deformation vectors. Lower values of `Alpha` lead to larger overall deformations, and vice versa. Parameter `Beta`, on the other hand, is a regularization parameter that tends to affect the degree of motion coherence of neighboring points. Large values of `Beta` will lead to greater motion coherence among neighboring points, and vice versa.

<br />
<p align="center">
<img src="images/scs03a.png" width='500  ' height='700'>
</p>
<br />

Now that we are acquainted with the overall layout of the module, let's start by looking at two example meshes. We can start by importing them using Slicer's data loading capabilities. Please click the `Data load` button, then navigate to the tutorial data folder, and load the following two meshes:

[You can get the zip archive of the sample data used in this tutorial from here.](https://app.box.com/s/vt4e7x59fxydjdp16evs4x9o4o31mhbe). 

* `A_J_.ply`: A/J inbred mice are widely used to model cancer and for carcinogen testing given their high susceptibility to carcinogen-induced tumors.
* `NOD_SHILTJ_.ply`: NOD/ShiLtJ inbred mice are widely used as a polygenic model for autoimmune type 1 diabetes.

<p align="center">
<img src="images/SCS04A.png" width='1280' height='600'>
</p>


 * If everything worked properly, you should observe something that looks like this:

<p align="center">
<img src="images/scs05.png" width='1280' height='600'>
</p>

* As can be seen above, the two meshes lie in arbitrary positions in 3D space. Contrary to other approaches present in the literature, `ALPACA` can deal with arbitrary starting points. 

* But let's see `ALPACA` in action. Please clear your scene using `Ctrl + W`. Then let's return to the `ALPACA` module and select those two meshes under the `Single alignment` tab. 

* We will also load the `*.fcsv` file containing the landmarks that we wish to transfer from the source to the target meshes.

<p align="center">
<img src="images/SCS06A.png" width='1280' height='600'>
</p>

* After selecting all four inputs, we can go ahead and press `Run subsampling`. 

* As seen below, `ALPACA` will print the number of points sampled in each mesh. It is essential to aim for `5000-6000` points per mesh, so the user has the option of pressing the `Run subsampling` as much as needed. A good follow-up exercise to this tutorial is to vary the number of sampled points per mesh and evaluating the impact it has on the performance of the method. 

* The `Run subsampling` button will also load a visual representation of the `Target` pointcloud into the 3D scene (in blue).

<p align="center">
<img src="images/SCS07A.png" width='1280' height='600'>
</p>

* Once we are satisfied with the number of sampled points, we can proceed with `rigid registration` steps of the pipeline by using the `Run rigid alignment` button. 

* This step will produce an output corresponding to the visual representation of the alignment between the `Source`(red) and `Target` (blue) pointclouds in the 3D scene. Please feel free to rotate those pointclouds in 3D space to make sure the alignment occured correctly.

<p align="center">
<img src="images/scs08a.png" width='1280' height='600'>
</p>

* Depending on the complexity of the structure of interest, it may be hard to tell if the pointclouds are properly aligned. For that reason, `ALPACA` offers the users the option of displaying the rigid aligned meshes. Please press `Diplay rigid aligned meshes`. You should observe something as seen below. Again, feel free to rotate the 3D surfaces to make sure they are properly aligned. 

* In our example case (mice), you will notice that even though we get a proper alignment between the two strains, the `A_J` mice have a downward curved face when compared to the `NOD` mice. Note how the nasal bones are distant from each other. For that reason, simply transferring the landmarks after the rigid registration step is unlikely to produce good results.


<p align="center">
<img src="images/scs08b.png" width='1280' height='600'>
</p>

* To further improve the quality of the alignment, we need to be able to deform the `Source` mesh to match the `Target` one. We can obtain the deformable alignment by pressing `Run CPD non-rigid registration`. Note that the non-rigid step is by far the longest (time) step of the pipeline. In modern laptops, this should take around 2 or 3 minutes. 

* You should get as an output the final landmark predictions in the form of `Fiducial` points. In the `Single alignment` tab, the output fiducials are not saved into a file. In part, this is because the role of the `Single alignment's` tab main role is to find the best combination of parameters necessary to transfer landmarks between specimens. These parameters can then be transferred to the `Batch processing` tab to process an entire specimen folder. 

<p align="center">
<img src="images/scs09a.png" width='1280' height='600'>
</p>

* Note that, as a final step the `Single alignment` pipeline, the user has the option of visualizing the registered `Source` model (i.e., deformed `Source` mesh). Simply press `Show registered source model` to obtain the deformed mesh (green). Note how the alignment of the nasal bone is much better than prior to the deformed step.

<p align="center">
<img src="images/scs10a.png" width='1280' height='600'>
</p>



## Batch processing 

* As mentioned in the prior section, the main purpose of the `Single aligmment` tab is to find the best combination of hyperparameters for the task, so that they can be applied to a large array of specimens. In `ALPACA`, the parameters used in `Single alignment` tab get transferred to the `Batch processing` tab once that tab gets selected. 

* Note that the `Batch processing` tab has much of the same elements as the `Single alignment` one. The main difference is the addition of a ` Target output landmark directory` box. Before an output directory gets selected, the `Run-autolandmarking` button cannot be pressed.

<p align="center">
<img src="images/scs11a.png" width='1280' height='600'>
</p>
* So, let's select an output folder! You should note that once the folder is selected, the `Run-autolandmarking` button becomes active. Once pressed, the analysis will proceed by iterating through all `*.ply` files contained in the `Target mesh directory`. Depending on the number of specimens, this will take a considerable amount of time. In most modern laptops, an average of `4-5min` per specimen should be expected. An output `*.fcsv` file will be produced for each specimen using the same name as the original `*.ply` files.



## Post-processing

Once a batch of specimens have their landmark positions predicted using `ALPACA`, the user has the option of further exploring the performance of the method. Here, we will investigate whether the skull shapes recovered using `ALPACA` correspond reasonably to what would be obtained by manual annotation. For this step, we will use a [small dataset of Gorilla skulls](https://app.box.com/s/a1eg0m2d5f41o6bbqa604tgz528xgsq2/) for which we have both manual annotations and automated predictions.

Please find the navigation drop-down menu and then click:

  * __SlicerMorph > Geometric Morphometrics > GPA__
  
<br />
<p align="center">
<img src="images/scap15_cropped.png">
</p>
<br />

* Once inside the `GPA` module, let's select the folder containing the landmark files and the output folder (in this case, the same folder) and then press `Execute GPA + PCA`.

<br />
<img src="images/scap16_anno.png">
<br />

* If everything loaded correctly, you should observe something like this:

<br />
<img src="images/scap17.png">
<br />

* To further facilitate our interpretation of the results, we will also load a surface model for the 3D views. Find `Setup 3D visualization` and select the _Gorilla_ skull mesh and landmarks that were downloaded using the `Sample Data` module in previous days. Remember that the `Sample Data` module will download all files to your Slicer cache folder.

<img src="images/scap18_anno.png">

Once all data visualization is set, we can now investigate whether the landmarking method (manual or automated) affects the shape estimate of each specimen. In order to do so, we will create a factor called `LM_method` using the `PCA Scatter Plot Options` and use that factor to color-code the specimens in the PCA plots.

* Under `PCA Scatter Plot Options`, type 'LM_method' in the `Factor Name` box and then press `Add factor data`

* Note that a new column was added to the table on the right corner of the screen.

<img src="images/scap19_anno.png">

* For the purposes of this exercise, you will have to type the factor data for each specimen. Note that the files have filenames that end on 'LM1' or 'Cranium'.
  * `LM1` refers to the manual annotation
  * `Cranium` refers to the ALPACA predicted landmarks.

* Once you finish typing the information, select 'LM_method' under the `Select factor` tab and then press `Scatter Plot`.

<img src="images/scap20_anno.png">

* If everything ran correctly, you should observe a new middle plot that should look like this:

<img src="images/scap21.png">

* The PCA plots reveal some interesting things about how the method performed in this small dataset.

  * First, the two distributions are largely overlapping and data points belonging to the same individual cluster together in multivariate shape space (`red boxes`)
  * Second, the two distributions also have similar means.

<img src="images/scap21_anno1.png">

* However, we can also observe some interesting differences:
  * Note the data point in red at the upper left corner of the plot (`red box`). This is a data point produced by the ALPACA method that has no near neighbor. This sample is a good candidate for further inspection for prediction errors.
  * Note also the four blue data points that are on the lower tail of the PC2 distribution (`red box`). These data points were manually landmarked and also have no near-neighbors in the automated dataset. What could explain such difference?
  
<img src="images/scap21_anno2.png">

* To explain such differences, let's look at what shape changes are produced along the PC2 axis. In order to do so, let's select the PC2 under `PCA visualization parameters`. 

<img src="images/scap22_anno.png">

* Using the provided sliders (highlighted), let's deform the 3D model in the direction of those four data points at the lower tail of the PC2 distribution.

<img src="images/scap23_anno.png">

* Note that specimens at the lower end of the PC2 distribution have elongated skulls. Given that information, it is likely that the parameters chosen in the `deformable registration` step of the `ALPACA` pipeline might need some fine tuning. 

## Final remarks

* As a final remark, we should note that the `ALPACA` module can also be used synergistically with other `SlicerMorph` modules. For example, users can use not only manually annotated landmarks in the `ALPACA` pipeline, but also include semi-landmarks that were sampled on the 3D surface using the `Spherical Sampling` lab. 

<img src="images/scap24.png">



### And that is it for now!














