#Mobile Visual Regression
### Visual Regression impliciations with Reg-Suit

Some of the potential problems with Reg-Suit using the git-hash plug-in are:

- Branches MUST merge parent into themselves before merging into parent, to update source code and to update reference image snapshot key
- The full visual regression suite MUST be run every time even a single image is to be updated
- If images are accidentally deleted (or the visual regression is only partially run) and the branch is merged, any branch made after the merge will now be comparing to an incomplete set of images
- The report is not interactive, and only displays failures, and passes and new images - this means that if an intended UI change was coded, reg-viz reports would still show it as a failure, because it is different from the reference image.
- Because the visual regression takes time, if one were to branch from the parent RIGHT after a merge request was approved, then the set of reference images on the branch would be outdated


### Running Reg-Suit Nightly on Main Branch
- Running visual regression nightly is a possibility, because CI can compare the current day's results to the previous day's results
- **However,** this means that code would have to be merged into main branch without running visual regression, and resulting git auto-merge can create expected differences which show on the report as failures
