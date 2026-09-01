# PathologyExtension

This is an extension module that adds a filtering operation to select the proper
magnification for an analysis running at a pariticular level of detail, out of a multi-level-of-detail, pyramidal whole slide
image.  This filter examines each layer in a DICOM-Wsi input image and copies over only images with the desired resolution. 

The desired resolution to pass through is selected by adding the following sections to the configuration YAML 
of a model pipeline that desires to use this filter. 
Note how the pipeline `execute` section calls the magnification extraction directly after the 
DicomImporter and before the model invocation:

execute:
- DicomImporter
- WsiMagnificationExtractor
- (insert your pathology model runner here) 
- DataOrganizer

The filter accepts parameters to select a desired magnification (e.g. 10x) out of a pyramidal image. 
The extractor then passes through only the matching image or images from the pyramid.  In the DICOM-WSI standard,  a separate file is created 
for each resolution level in 
the original image.  
With the example parameters provided below, this extraction will pass only source images within the 
specified magnification range of 10x +/- 2x. In other words, the resultsing outputs will have the form 8x < image(s) < 12x. 

WsiMagnificationExtractor:
- target_magnification: 10
- magnification_tolerance: 2.0
- meta: 
-  mod: 'sm'


