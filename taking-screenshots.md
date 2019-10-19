#Mobile Visual Regression
### Taking Screenshots
There are **two common methods** to take automated screenshots of the iOS Simulator:

- **XCUIScreenShot (preferred method)**:
  - Screenshots are taken during the UI Tests
  - Locally, the screenshots are stored in the specified source_root environment variable specified in the UITesting Scheme
  - The functions to take, and name a screenshot in Swift is: 

  ```swift
  public func takeScreenshot(name: String) {
    let myImage = App.windows.firstMatch.screenshot().pngRepresentation
    let originUrl = URL(fileURLWithPath: ProcessInfo.processInfo.environment["source_root"]!).appendingPathComponent("\(name).png")
    
    do {
        try myImage.write(to: originUrl, options: .atomic)
    } catch {
        XCTAssert(false, "Was not able to save screen capture!")
    }
  }

  public func takeScreenshot(afterExistenceOf element: XCUIElement, name: String) {
    XCTAssert(element.waitForExistence(timeout: SHARED_TIMEOUT), "takeScreenshot: timed out waiting for element to exist!")
    takeScreenshot(name: name)
  }
  ```


> Call Using `takeScreenshot("imageName")`, will generate `imageName.png` when the test is run, and the image dimensions will be the dimensions of whatever device the simulator is tested on



- **Fastlane Snapshot**:
	- Set up Fastlane Snapshot by cd'ing into project directory and running `fastlane snapshot init`. A `Snapfile`, and `SnapshotHelper.swift` file will be generated in your project directory. `SnapShotHelper.swift` contains the function to take screenshots, and the `Snapfile` contains the configurations for the screenshots. The `SnapshotHelper.swift` file should be moved into your UI Test folder.
	- `snapshot("Login-Page”)` is the format on taking screenshots within XCUI Tests. Wherever this method is called, snapshot will store a screenshot of the current UI State, save it to the specified directory in the snapfile, and name it according to the parameters passed into the function.
	- The code `snapshot("test1")` will result in a screenshot of wherever the simulator was when this code was hit in the UI Test. The resulting image name will be `test1.png`, after `fastlane snapshot` is run.
	- After UI Tests are built and the code for creating snapshots is added, cd into project directory and run ```bundle exec fastlane snapshot```



###The Snapfile

The Snapfile is required for using Fastlane Snapshot to take screenshots. It can also be used to run the UI Tests on CI. The Snapfile allows the scheme, devices, languages, etc. to be specified easily. 

Note: The Snapfile is particularly useful when running in CI, because running `bundle exec fastlane snapshot` will run the UI tests on all specified devices in all specified languages, and will thus generate all the required screenshots (even you used `snapshot("image")` or `takeScreenshot("image")`, the Snapfile will still generate all screenshots).

- The Snapfile contains parameters to change:
	- Scheme
	- Devices
	- Languages
	- Output directory
	- Deleting previous screenshots
	- iOS Version
	- Erasing/Resetting the simulator


>To get a clean status bar in the screenshots, SimulatorStatusMagic can be used, or screenshots can be trimmed (more on this later).
>
>Detailed informations about the Snapfile configuration and Snapshot setup is available at <https://docs.fastlane.tools/actions/snapshot/>
