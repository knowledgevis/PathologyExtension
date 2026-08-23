# PathologyExtension

This is an extension module that adds a filtering operation to select the proper
magnification for an analysis out of a multi-level-of-detail, pyramidal whole slide
image.  This filter examines each layer in a DICOM-Wsi input image and copies over only the desired resolution. 

The desired resolution for an MHub model is selected by including the following sections to the configuration of a model that desires to use this filter. Note how the pipeline calls the magnification  extraction directly after the 
DicomImporter and before the model invocation:

-------- 

execute:
- DicomImporter
- WsiMagnificationExtractor
- <Your pathology model here> 
- DataOrganizer

# parameters to set desired magnification (e.g. 10x).  With these parameters, 
# this extraction will pass only source images within the 
# specified magnificato range 8x < image < 12x. 

WsiMagnificationExtractor:
    target_magnification: 10
    magnification_tolerance: 2.0
    meta: 
      mod: 'sm'


