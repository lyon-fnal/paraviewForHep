# What is ParaView?
(Adam Lyon, October 2015, ParaView 4.4)

[ParaView](http://www.paraview.org) is a 3D visualization application based on the [VTK](http://www.vtk.org) (Visualization Toolkit) library. To me, its important features are:

* Based on experience and research from the Computer Science Visualization community and the Supercomputing community.
* Extremely responsive scene manipulation (rotate, pan, zoom). 
* Very easy to overlay data from multiple sources (and apply transformations if necessary). 
* Many tools for visualization of scalar and vector fields (using, among other techniques, arrow glyphs, streamlines, heat maps, volume rendering).
* Easy slicing, cutting, and applying thresholds.
* Animation.
* 2D plots made from aspects of the 3D data.
* Deep python integration, including writing your own pipeline sources and filters as well as scripting the application.

ParaView is mainly authored and maintained by [Kitware](http://www.kitware.com), a company committed to open source platforms. 

# Obtaining ParaView

ParaView is available on the Mac, Windows and Linux platforms. The download page is at [http://www.paraview.org/download/](http://www.paraview.org/download/). ParaView should work out of the box without having to install any dependencies. If you really need to, you can build ParaView from source as described [here](../misc/build.md).

# Learning ParaView

ParaView is a very capable and somewhat complicated application with a learning curve. There are, however, many resources for learning the system. 

## Manual

The most important documentation from Kitware is the ParaView Guide in PDF or print form. The guide is the main manual for ParaView. A free community edition is available from [http://www.paraview.org/paraview-guide/](http://www.paraview.org/paraview-guide/). There is also a print version that contains several more examples and use cases (the Fermilab library has a [copy](http://www-spires.fnal.gov/spires/find/books/www?cl=QA76.9.I52.A95::2015)). Certainly you should download and skim the manual, but it may be easier to start learning from a tutorial. 

## Tutorials

Kitware has an official tutorial at [http://www.paraview.org/Wiki/The_ParaView_Tutorial](http://www.paraview.org/Wiki/The_ParaView_Tutorial). PDF of the tutorial document and links to the necessary data files are on that page. This tutorial is quite well written and extensive. It is a good place to start.

Kitware has a wealth of information on its [Wiki](http://www.paraview.org/Wiki/Main_Page), about both [ParaView](http://www.paraview.org/Wiki/ParaView) and [VTK](http://www.vtk.org/Wiki/VTK).

An excellent tutorial explaining how to handle scalar and vector fields in the context of climate science is at [https://www.dkrz.de/Nutzerportal-en/doku/vis/sw/paraview](https://www.dkrz.de/Nutzerportal-en/doku/vis/sw/paraview). Download the PDF and the data needed for the tutorial. The web site can be quite slow and the data download is very large. If you have trouble downloading the materials, let Adam know and he can send you his copy. 

A more simple tutorial is from Boston University at [http://www.bu.edu/tech/support/research/training-consulting/online-tutorials/paraview/](http://www.bu.edu/tech/support/research/training-consulting/online-tutorials/paraview/). It is based on a rather old version of ParaView, but it is still relevant. They also have a tutorial on VTK itself 

A good university course with a wealth of online material is at [http://cs.unc.edu/~taylorr/Comp715/](http://cs.unc.edu/~taylorr/Comp715/). Of special interest are a [tutorial on animation](http://cs.unc.edu/~taylorr/Comp715/ParaView3_8_1_Tutorial.html) and a course [schedule](http://cs.unc.edu/~taylorr/Comp715/schedule.html) that contains many links to resources explaining visualization concepts. 

The US Department of Defense has a large amount of information at [http://daac.hpc.mil/software/ParaView/](http://daac.hpc.mil/software/ParaView/). 

Finally, the visualization group at the Argonne Leadership Computing Facility has a very good tutorial at [https://www.alcf.anl.gov/user-guides/vis-paraview-red-blood-cell-tutorial](https://www.alcf.anl.gov/user-guides/vis-paraview-red-blood-cell-tutorial) creating a complicated pipeline in ParaView. 