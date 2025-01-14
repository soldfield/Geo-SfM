# Metashape Tutorial

This tutorial (and exercise) features the following learning goals:
- You will **learn the basics of Agisoft Metashape**
- You will **use Agisoft Metashape to digitise samples and/or outcrops**

```{admonition} Support
:class: warning
Please note that **we only provide feedback and support for students enrolled in the course at the University Centre of Svalbard**.
```

## 3D model reconstruction with Agisoft Metashape

````{admonition} Creating high-quality models the USGS way
:class: suggestion

The United States Geological Survey {cite}`overProcessingCoastalImagery2021` has put together an excellent tutorial on the use of Agisoft Metashape for creating high resolution digital elevation models.
Although digital elevation models are but one of many desired products, the same guide can be used for the creation of high resolution and properly processed DOMs.
USGS guidelines and the quick start guide to processing (Table 1) can be found [here](https://pubs.usgs.gov/of/2021/1039/ofr20211039.pdf), and should be used supplementary to this tutorial.
```
https://pubs.usgs.gov/of/2021/1039/ofr20211039.pdf
```
````

In this session we will learn how to use Agisoft Metashape.
This is an established SfM photogrammetry package and is often used in the Arctic Geology department at UNIS to create digital outcrop models (DOMs) and hand-sample models (HSMs).
This session introduce basic processing through the graphical user interface through use of a standardised data set.

```{admonition} Software version
The following tutorial assumes the use of **Agisoft Metashape version 1.7.x.**

To see whether you are indeed running the correct version proceed to *Help/About Metashape...* in the menu bar.
This should provide you with the **major release**, *minor*, and patch (**1**.*7*.x) version number, as well as the build.

If the version does not match the version listed above, please either
- update the installation; or
- contact the Seismic Lab software responsible (Peter Betlem)
```

```{admonition} Think before doing...
:class: warning
Always remember that quality comes at a cost, in this case as a computational cost (i.e., time) and storage cost (i.e., size).

In processing this is the key dilemma, and requires you to think about the output before you start.

*Is it really necessary to generate a 500 GB model for a 1 by 1 square metre area, considering that your max upload limit for an assignment is only 200 MB?*
```

```{admonition} Further reading
:class: tip
The guidelines below are based on the official Agisoft *3D model reconstruction* tutorial.
This tutorial, as well as more in-depth help, can be visited [here](https://agisoft.freshdesk.com/support/solutions/articles/31000152092).
```

````{margin} Network processing

For those present at UNIS and following this tutorial as part of the UNIS AG-222 Course, please enable network processing.
Proceed to *Tools* in the menu bar and click on *Preferences...*.
Then proceed to *Network* and check the *Enable network processing* setting.

The following parameters should be used to connect to the server:

Host name:
  ~ svalbox

Root:
  ~ \\svalbox\metashape-processing$

Port number:
  ~ 5840

Whenever you want to use the network for processing, you will have to place your project within the *Root* folder specified above.
````

### A standardised project environment

We will be using a standardised project environment throughout this course.
It is recommended to use this setup (or something similar) throughout all future projects as well.
The use of this standardised project setup is furthermore a requirement for digital models to be uploaded to [the Svalbox DB](http://svalbox.no).

Proceed by creating the following project_directory in the folder where you have all your projects..
Herein folders are named without extensions, and filenames are given extensions.
Text between {} indicates variable names.

```
{project_directory} (The folder with all files related to this project)
|   overview_img.{ext}
|   description.txt
├───config (where you place your configuration files)
        {cfg_0001}.yml
        {cfg_0002}.yml
        ...
├───data (where you unzipped the files to)
├───────100MEDIA (The folder in which all the images reside)
|           {img_0001}.{ext}
|           {img_0002}.{ext}
|           ...
├───────101MEDIA (The folder in which all the images reside)
|           {img_0001}.{ext}
|           {img_0002}.{ext}
|           ...
|       ...
├───────199MEDIA (The folder in which all the images reside)
|           {img_0001}.{ext}
|           {img_0002}.{ext}
|           ...
├───────gcps
|           (We'll get back to this in a later session)
├───export (where you place export models and files to)
        ...
└───metashape (This is where you save your Agisoft Metashape projects to)
        {metashape_project_name}.psx
        .{metashape_project_name}.files
        {metashape_project_name}_processing_report.pdf
        (optionally: {metashape_project_name}.log)
```

The standardised project structure (as we will see later on) is also used for automated processing.

### Photo set

Having created the standardised project structure, proceed with extracting all your taken images to the following directory:

```
{project_directory}\data\100MEDIA
```

```{admonition} More than 999 images?
:class: suggestion

In case your project contains more than 999 images, make sure to utilise multiple media folders in the *data_directory*.
In other words, create as many *{XXX}MEDIA* subdirectories as needed, and limit their contents to 999 images.
This is especially useful when dealing with non-unique filenames, as the different directories prevent files being overwritten.
```

````{admonition} Want to follow along without your own data?
:class: suggestion

Go ahead and download the package, then extract the archive's contents as per the above.

```
URL:
```

````

### Adding photos

Next we'll be adding the photos to Agisoft Metashape.
This assumes
To do this, proceed to and click on *Add photos...* from *Workflow* in the *menu bar*.
In the dialog that pops up, browse to the *project_directory/100MEDIA* folder that you created in the previous section.
Select all files to be processed.
These should now show up in a *chunk* with *cameras* in the *Workspace* panel.
Verify that this is indeed the case by double clicking one of the *cameras* in the *Workspace/Chunk/Cameras* panel.

```{admonition} Save often!
:class: tip
It is important to save your work often.
Make a habbit of saving at least after every step.
To do so, proceed to *Save as...* under *File* in the menu bar, and save your project in the *project_directory/metashape* directory that you created when extracting in data.
```

### Analyze photos

Before actual processing begins, we need to analyse the imported photos.
This is done by right clicking any of the photos in a *Chunk*, then selecting *Estimate Image Quality...*, and select all photos to be analysed, as shown in {numref}`analyze_process`.

```{figure} assets/analyze_process.gif
:name: analyze_process

Estimating image quality in the Metashape GUI.
```

One can then open the *Photos* pane by clicking *Photos* in the *View* menu.
Then, make sure to view the details rather than icons to check the Quality for each image ({numref}`analyze_select`.)

```{figure} assets/analyze_select.gif
:name: analyze_select

Showing the image quality and disable photos that do not meet the requirement in the Metashape GUI.
```

Then, filter on quality and *Disable* all selected cameras that do not meet the standard.
Agisoft recommends a *Quality* of at least 0.5.

### Aligning photos

With the photos now imported into Metashape and analysed, we can proceed with the alignment process.
This process goes through all images in the project and tries to identify *common features*.
In Metashape this first requires the estimation of camera positions for each photo, which are then used to build a *sparse cloud*.
Select *Align Photos...* in the *Workflow* menu.

A {ref}`dialog <align_photos>` will pop up with several parameter options.
Most important here is the *Accuracy* parameter, which governs whether your photos are down-sampled before alignment.

For now we'll skip the other parameters; just make sure to deselect *Reference preselection*, *Guided image matching*, and *Adaptive camera model fitting*.
Keep *Generic preselection* and *Exclude stationary tie points* selected.

After clicking *OK*, Metashape starts aligning your photos.
This may take a while, but assuming there is sufficient overlap between the data, a *sparse point cloud* will be shown on the screen (in the *Model* tab) once processing is done.
If not, one can select this by clicking the four-dotted icon in the menu.

```{figure} assets/3a24665c.png
:name: align_photos

The *Align Photos* dialog after opening it from the *Workflow* menu.
```

```{admonition} Down-sampling
Down-sampling is the process in which you combine parts of a data set, resulting in the loss of knowledge.
For example, down-sampling a 1000x1000 pixel image to a 100x100 image results in a factor 100 compression of data (= easier to process), but you also lose the initial resolution!

Depending on your computer specifications, you'll have to weigh computational time versus output quality.

Give it a shot, and compare the photo alignment results with *medium* vs *high* processing accuracy.
```

(content:tutorial:improve)=
### Improve alignment step: Error Reduction-Optimization and Camera Calibration


```{admonition} Ground control points
:class: warning
Additional steps are needed to ensure ground control points and checkpoints are adequately integrated into the model.
We will discuss what ground control points are and what they can be used for in [](content:gcps).

Those implementing ground control points, please refer to the USGS manual for the correct sequence of steps and optimizations.
```

Although the photos have now been aligned, it is important to further optimize the cameras.
This is done by selecting *Optimize Cameras* from the *Tools* menu.
A {ref}`dialog <optimize_cameras>` will pop up with several parameter options pre-selected.
Most important here is to at least select make sure that the *Estimate tie point covariance* is enabled.

```{figure} assets/291baf87.png
:name: optimize_cameras

The *Optimize Camera Alignment* dialog after opening it from the *Tools* menu.
Make sure to enable *Fit f*, *Fit k1-k3*, *Fit cx, cy*, *Fit p1-p2*, and *Estimate tie point covariance*.
```

Once processing is done, change the model view to show the Point Cloud Variance by following the instructions given in {ref}`dialog <point-cloud-variance>`.
Lower values (=blue) are generally better and more constrained.

```{figure} assets/view_tie_point_covariance.gif
:name: point-cloud-variance

Changing the view of your points and model to show errors and other things.
Here we change our view to show Spare Point Covariance instead of RGB coloring.
```

As the sparse cloud is the very first step in a long processing chain, we want to optimize it as much as possible.
After all, bad quality in usually equals bad quality out.
We will now conduct several optimizations to improve quality of the sparse cloud, implementing USGS {cite}`overProcessingCoastalImagery2021` recommendations that aim to reduce reconstruction uncertainty, improve projection accuracy, and lower the overall reprojection errors.
Unlike shown below, the project sometimes benefits by having each of the steps repeated multiple times, rather than just once.
Keep in mind, however, that running too many optimizations may also result in over-fitting of the data.
That means that the model is no longer based on real input, but rather on the processing parameters (= not real!).

````{admonition} Make a backup for every step
:class: tip
Whenever you are about to perform a destructive action (i.e., "deleting" stuff), make sure to make a backup of the current state first.
In Metashape this can be done by right clicking an object (e.g., Chunk, Points, etc.) and selecting the *Duplicate...* option.
It usually takes a bit of time to complete, but the cost of not doing so is usually much, much higher!

```{figure} assets/duplicate_chunk.gif
:name: duplication

Making a backup duplicate of Chunk 1. To keep things simple, make sure to rename your copies; else you may end up with a Copy of Copy of Copy of Something...
```
````


#### Filtering by Reconstruction Uncertainty

Once you're ready (ahem, make a duplicate/backup first!), filter your Sparse points by their Reconstruction Uncertainty.

```{figure} assets/reconstruction_uncertainty_filter.gif
:name: filter_reconstruction_uncertainty

The *Gradual Selection* dialog after opening it from the *Model* menu, here showing selections that are possible for the Sparse Point Cloud.
A good value to use here is 10, though make sure you do not remove all points by doing so!
After clicking OK, the left corner shows you how many points there are in total, followed by the number currently selected.
A rule of thumb is to select no more than two-thirds to half of all points, and then delete these by pressing the Delete key on the keyboard.
```

After deleting the selected points, it is important to once more optimize the alignment of your points.
Do so by revisiting the [](content:tutorial:improve) section.

#### Filtering by Projection accuracy

This time, select the points based on their Projection accuracy, aiming for a final Projection accuracy of 3.

```{figure} assets/f7c6c589.png
:name: filter_projection_error

The *Gradual Selection* dialog after opening it from the *Model* menu, here making a selection based on the projection accuracy parameter.
A good value to use here is 3, though make sure you do not remove all points by doing so!
After clicking OK, the left corner shows you how many points there are in total, followed by the number currently selected.
```

Keep in mind that not all projects can tolerate the removal of points below 5, which is no problem as long as you document the values you have used.
After deleting the selected points, it is important to once more optimize the alignment of your points.
Do so by revisiting the [](content:tutorial:improve) section.

#### Filtering by Reprojection Error

Up next is to filter the points by their reprojection error, see {ref}`dialog <filter_reprojection_error>`.
A good value to use here is 0.3, though make sure you do not remove all points by doing so!
As a rule of thumb, this final selection of points should leave you with approx. 10% of the points you started off with.

```{figure} assets/5fba8b6a.png
:name: filter_reprojection_error

The *Gradual Selection* dialog after opening it from the *Model* menu, here making a selection based on the reprojection error parameter.
After clicking OK, the left corner shows you how many points there are in total, followed by the number currently selected.
```

After deleting the selected points, it is important to runa final *Optimize alignment* step on your points.
Do so by revisiting the [](content:tutorial:improve) section.

```{admonition} Keep your duplicates
:class: tip

It is always a good idea to keep your duplicate backups of previous steps until you are fully done with them.
They allow you to go back and write down important processing parameters, such as how many points were filtered during each step and which cutoff values you used during processing.
Only once completely done should you remove these earlier attempts.
```

### Build Dense Cloud

Based on the estimated camera positions, we can now estimate a *dense point cloud* by calculating depth information for each image.

Select *Build Dense Cloud* from the *Workflow* menu.
{ref}`Once again you will be asked about the desired accuracy/quality <dense_cloud>`.
Keeping in mind what we discussed above, make sure to select *Medium* for the quality.

Also open up the *Advanced* section, and set *Depth filtering* to *Mild*.
**Make sure to always enable *Calculate point confidence***

*Reuse depth maps* can only be selected if depth maps have been previously calculated for the specified quality.

```{figure} assets/d2a861b0.png
:name: dense_cloud

The *Build dense cloud* dialog after opening it from the *Workflow* menu.
```

Once processing is done, you'll be able to show the *dense point cloud* on the screen in the *Model* tab.
If not, one can visualise the *dense point cloud* by clicking on the nine-dotted icon in the menu.
Alternative, you could also visualise the point confidence.
Do so by clicking the gray triangle next to the nine-dotted icon and selecting *Dense cloud confidence*.
The colour coding (red = bad, blue = good) shows where the best confidence is found.

```{admonition} Point confidence
*Point confidence* shows how accurate a given point in the dense cloud is.
This is key to estimate whether the part we are interested in can be scientifically used for measurements and characterisation based on predefined criteria.
```

```{admonition} Dense cloud quality
:class: tip
While this tutorial suggests *Medium* to be used for the quality parameter, you should feel free to change this depending on the computational power at your disposal.
```

#### Selection by point confidence

```{admonition} Save time - make backups
:class: tip
Make sure to always either backup your data before playing with it.
This can be done by either backing up the entire project (*Save as...*) or by making a local copy of the data within the project.
To do the latter, right click on the *Workspace/Dense Cloud (... points)* and select *Duplicate...*.
Following alterations to the duplicated data set, you can always switch back to the original by right clicking *Workspace/Dense Cloud (... points)* and *Set As Default*.
```

There are various ways of cutting off parts of your Dense Cloud.
However, not all of them are scientifically repeatable - because let's be honest, would you be able to exactly tell which of the points you removed from the data? No.

Luckily, Metashape allows selection and deletion of points by *Point confidence*.
Proceed to *Tools/Dense Cloud* in the menu and click on *Filter by confidence...*.
The dialog that pops up allows you to set minimal and maximal confidences.

For example, try setting *Min*:50 and *Max*:255 to only show the Dense Cloud points wth the highest confidence.
After looking at the difference, reset the filter by clicking on *Reset filter* within the *Tools/Dense Cloud* menu.

We will now use the method outlined above to filter out the lowest confidence interval (in this case, set *Min*:0 and *Max*:5).
Proceed by selecting all the points shown in the *Model* tab.
Do this by enabling the *Rectangle Selection* tool, found next to the mouse pointer icon in the menu.
Then drag a rectangle selection and include all to-be-selected points currently visible.
Delete the selected points by hitting the *Del(ete)* key on the keyboard.

Now reset the filter, and you'll see that just the high-confidence part remains. :)

```{admonition} Scientific repeatability
**Always write down the chosen confidence interval** - repeatability depends on it!
```

### Building a mesh

We can use the dense point cloud to generate a polygonal mesh model.
While generating the dense cloud, Agisoft Metashape simultaneously generated a set of depth maps (if chosen to save to the project).
This is important as we can decide which of the two to use for meshing.
Depth maps may lead to better results when dealing with a big number of minor details, but otherwise dense clouds should be used as the source.

After selecting *Build Mesh* from the *Workflow* menu, you will be able to chose either in the {ref}`dialog <build_mesh>` that pops up for *Source data:*.

```{figure} assets/9b4ac0b8.png
:name: build_mesh

The *Build mesh* dialog after opening it from the *Workflow* menu.
```

Other important parameters here are the *Quality* and *Face count* parameters.
These govern the quality of the generated mesh.
Remember, however, that quality comes at a cost, so best to stick to *Medium* or lower to manage the exercise within the allocated of time...

Finally, feel free to enable the *Calculate vertext colors* to provide the mesh with some colour as well, otherwise it will be shown as a purple mesh.

Once generated, we can take a closer look at the mesh by clicking on the *Tetrahedral* icon next to the nine-dotted icon.
Clicking on the gray triangle next to it, you see that the *Model Textured* is still grayed out.
The next step involves generating a texture and placing this "image" onto the mesh - after which the *Model Textured* becomes selectable.

```{admonition} Depth maps
:class: tip
If depth maps do exist, and you decide to use them as the source data, then make sure to enable *Reuse depth maps* to save computational time!
```

##### Filtering the mesh

Sometiems your mesh/model features blobs that are not connected to the main model.
These can be easily (and scientifically!) removed through use of the *Connected component filter*, see {ref}`dialog <filter_connected_component_size>`.

```{figure} assets/filter_connected_component_size.gif
:name: filter_connected_component_size

Filtering the mesh based on the connected component size.
This is a percentage of the largest component, which by default is 100.
```

```{admonition} Select the model first!
:class: warning
The option may not be available if you have not selected the mesh/model data first, or not in the model view panel.
```

#### Texture building

We can build the textures by clicking on the *Build Texture* command from the *Workflow* menu.
While the dialog has many different parameters, the most important are highlighted in {numref}`build_texture`.

```{figure} assets/7237597c.png
:name: build_texture

The *Build texture* dialog after opening it from the *Workflow* menu.
```

Here *Texture size/count* determines the quality of the texture.
Keep in mind that anything over 16384 can very quickly lead to very, very large file sizes on your harddisk.
On the other hand, anything less than 4096 is probably insufficient.
For greatest compatibility, keep the *Texture size* at 4096, but increase the count to e.g. 5 or 10.

```{admonition} Publishing on V3Geo or Sketchfab
:class: tip

Publication guidelines for V3Geo specify that the *Texture size* must always be 4096.
In addition, no fewer than 10 *Texture count* must be used.
It is recommended to follow this even for SketchFab, as this setup generally results in a higher-quality look of the published model.
```

### Generating a Tiled Model

Sometimes the need arises to not only build a mesh, but also a *Tiled Model*.
We can do this by selecting *Build Tiled Model* from the *Workflow* menu.
Once again we can select the parameters for the processing step through the {ref}`dialog <build_tiled_model>` that popped up.

The most import parameter here is the *Pixel size (m)*, which should never be set than lower than the pixel size available to the model (= the default value shown when opening the dialog).

```{figure} assets/6c44a1dc.png
:name: build_tiled_model

The *Build Tiled Model* dialog after opening it from the *Workflow* menu.
```

### Documenting processing parameters

While each of the previous steps has been important in generating the models, ***perhaps the most important aspect of processing is to document the taken processing steps and their parameters***.
Metashape automatically keeps track and stores all *Workflow* actions that the project has undergone.
There are two ways of documenting and showing these parameters.

```{admonition} What about manual alterations?
:class: warning
Sadly, the processing report does not include manual changes made to e.g. the point or dense cloud.
This means that to be truly *scientifically grounded* and *repeatable*, you should refrain from doing such alterations manually.
Instead, rely on e.g. the point confidence to make edits (and always report the chosen parameters).
```

#### Processing report

The first, and perhaps most important, is to generate a processing report.
Proceed to *File/Export* in the menu bar and select *Generate Report...*.
Provide the desired *Title* and description, then proceed by clicking *OK*.
Store the report alongside the Metashape project in the same directory.

```{note}
The procedure listed above should **always** be implemented in your workflows, with the output stored together with the raw data (e.g., images) and Metashape project files.
Make this a habit :)
```

```{admonition} Exercise 2
:class: tip
You'll find key metadata within the generated processing_report.pdf that has to be submitted as part of the second session's assignment.
Have a look and familiarise yourself with it.
```

#### In-programme parameters

One does not always need to export the processing report to obtain the overview of parameters.
Handily, Metashape also provides an overview within the *Workspace* panel after having selected an item in the chunk.
An example of this is depicted in {numref}`internal_parameters`.

```{figure} assets/5201e3e2.png
:name: internal_parameters

An example of the internal *Workspace* panel, including the parameters and processing metadata that is available from within the Metashape project interface.
```

```{tip}
The above is just a very generalised approach to SfM-photogrammetry that outlines the basic steps to be taken from photo acquisition to model generation.
In many cases the chosen parameters can (and should!) be changed to match the given circumstances.
That said, the *Photo alignment* step should always be conducted at the highest setting possible (considering computational power available).
Furthermore, one should ***always document each and every processing step performed*** - manually cutting and editing part of the model is therefore *not* encouraged as this remains (for now) difficult to document.
```
