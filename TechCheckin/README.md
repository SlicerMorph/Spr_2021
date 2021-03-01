## Tech Check-in Instructions:


1. Download the latest stable (**Slicer 4.11.20210226, revision 29738**) for your operating system from https://download.slicer.org/ (please make sure you are using this specific version not an earlier or a later version, this is to make sure everyone is using the same version). This is also the same version installed on SlicerMorphCloud. 
2. Open extension manager, go to “Install Extensions” tab and search for SlicerMorph and click *install*.
Wait until all 9 extensions are shown under “Manage Extensions” tab, and then click Restart
After the restart, open the module list and see that SlicerMorph is now listed.

<img src="extension_manager.png">

3. Click on ALPACA (SlicerMorph->Geometric Morphometrics), this will download and install an external python library called *Open3D*. It is a large library and it may take 5-10 minutes to install, during which Slicer will look like it stalled. Be patient. [Mac users who are using MacOS earlier than 10.15, need to follow this instruction in their Python console.](https://discourse.slicer.org/t/cant-load-open3d/12950/6?u=muratmaga) 

4. Go to Auto3Dgm, which will also download an external python library called *Mosek*. 
Mosek is a proprietary library and you will need a license to run. You can either request a free academic license for a year, or ask for a trial license for 30 days. Follow the license installation instructions at https://toothandclaw.github.io/installations. **HINT:** you can actually search for a file called *save_mosek_license_here* and copy the license file (mosek.lic) sent to you to the same place as this file. 

5. If you haven't encountered any issues, at this point you should be set. Please type these comments to your python window to double-check (there should no error messages):

  ```import open3d```
  
  ```import pycpd```
  
  ```import mosek```
  
If you end up getting error message about any of these libraries being missing, then you can try manual installatio (again type these to your python console):

```pip_install("open3d==0.10.")```
  
  This will manually install the open3d, for other libraries change the open3d to whatever is missing. 

<img src="python_console.png">

## How to update SlicerMorph (or any other extension?)
SlicerMorph is constantly improved with new features and functionality. To make sure you are using the latest version of SlicerMorph, go to the Extension Manager, click on Manage Extensions, and then click **Check for Updates**. Any installed extension with an update will now have a button that says "Update". Update the extensions you want, and restart Slicer for changes to take effect. This feature is only available for stable releases (doesn't work preview versions). 

## SlicerMorphCloud instructions
We will email you an URL and a password for you to use our SlicerMorphCloud instance through out the workshop. All you need is a recent browser (Chrome, Firefox or Edge). However, for better experience (particularly if your connection is slow), we suggest using TurboVNC client. It is freely available at https://sourceforge.net/projects/turbovnc/files/2.2.6/  

