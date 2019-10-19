#Mobile Visual Regression
### The Ideal Visual Regression Tool

An ideal visual regression tool would allow us to:

- Have multiple sets of reference images for different branches
- Individual Images themselves are merged, and updated, rather then the WHOLE SET of images moving around
- Have a UI that allows images to be accepted as the reference (merged in), or report failure images
- Do comparision in the cloud, rather then on local machines because download and upload times for the whole image set is very time consuming
- Have a command line tool that can accept/deny images
- The tool should be able to return an error code 1, not a 0 when run on CI so we can get a definite failure
- The tool should only give the error if a NON-APPROVED diff image is generated
- The diff image would give more details about the failing elements, and would not be so sensitive that it would give false positives
- Upload/Download/Delete/Change Reference images from S3 through the UI

![](https://image.ibb.co/dbzJ5H/Screen_Shot_2018_03_19_at_9_57_04_AM.png)
>In the report, there should be an interactive Accept/Reject button that allows the addition of the image to the set of reference images for that branch
