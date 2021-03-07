# Spring 2021 SlicerMorph 3D Morphometrics Online Short Course 
Lectures, labs and all documents associated with Summer 2020 workshop.
<img alt="SlicerMorph logo" width="358" height="256" src="https://github.com/SlicerMorph/SlicerMorph.github.io/blob/master/SlicerMorph_Logos/SlicerMorph_Final_Logos-V2.jpg">

# Day 3 Contents

**SLICERMORPH and Geometric Morphometrics**
*	[Generalized Procrustes Analysis (GPA) in SlicerMorph](https://github.com/SlicerMorph/Spr_2021/blob/master/Day_3/GPA/GPA.md)

## Introduction to Semi-Landmarks:

Criteria for landmarks to be homologous and reproducible may result in very sparse data from images. This may result in a very crude representation of underlying anatomy, and may be insufficient to capture shape changes (e.g., along curves or smooth surfaces in 3D images; think of ankle bones, or ribs of mammals). One goal of our project was to provide some simple ways for users to generate more points on 3D surfaces. These points are not biologically homologous, but sufficient attention is paid into their creation, they can be **geometrically homologous**. Each method comes with its on drawbacks and advantages based on the tradeoffs made between computation time, equidistant point spacing, sampling extent, and homology of the points that should be evaluated based on the application. That why we have multiple methods. 

## Resampling curves (aka curve-based semiLMs)
The simplest support for sampling points on a mesh is a newly implemented method to resample a curve at equidistant points snapped to a 3D model surface. This has now been integrated into the core of Slicer and is available in the 'Resample' menu of the Markups module for open and closed curves. You have already seen this as part of the [**Markups** module in Day 2](https://github.com/SlicerMorph/Spr_2021/blob/master/Day_2/Markups/Markups.md#3d-models-and-curve-based-semi-landmarking) 


## [Patch-based semiLMs:](https://github.com/SlicerMorph/Spr_2021/blob/master/Day_3/Patch-based_semiLMs/Patch-based_semiLMs.md)

## [Pseudo-landmark sampling:](https://github.com/SlicerMorph/Spr_2021/blob/master/Day_3/PseudoLMGenerator/PseudoLMGenerator.md)

## [MarkupEditor:](https://github.com/SlicerMorph/Spr_2021/blob/master/Day_3/MarkupEditor/MarkupEditor.md)
