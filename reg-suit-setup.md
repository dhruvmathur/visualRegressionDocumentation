#Mobile Visual Regression
### Initial Reg-Suit Set-Up

>See the Reg-Suit repository for more details about what Reg-Suit is: <https://github.com/reg-viz/reg-suit>

**Local** Installation Procedure:

1. `npm init` - to create package.json
2. `npm install –g reg-suit` – to install reg-suit globally on machine
3. `npm install -D reg-suit` - to install reg-suit into current working directory

    N.B.: As of this writing, Reg-Suit will not work unless installed globally and in current working  directory

4. `reg-suit init` - to configure reg-suit (involves specifying working directory, actual image directories)
	- The `reg-suit init` set up is used to generate the regconfig.json file. It will require input from the user for what plug-ins should be installed. For our purposes, only the *Git Hash* and *Publish to S3* plug-in need to be installed. Specify a S3 bucket to place the images in.
5. `reg-suit run` will fetch, compare and publish images (do the visual regression)

**If Using git-hash plug in, your reg-config file should look like:**

```js
{
	"core": {
    "workingDir": ".reg",
    "actualDir": "directory_contains_actual_images",
    "threshold": 0,
    "thresholdRate": 0.02,
    "ximgdiff": {
     "invocationType": "client"
    }
},
	"plugins": {
    "reg-keygen-git-hash-plugin": true,
	"reg-publish-s3-plugin": {
	  "bucketName": "visualregression"
	}
 }
}
```

>If the `"reg-keygen-git-hash-plug-in":` key has `{}` for it's value instead of `true`, replace the brackets with `true`.

**If Using simple-keygen plug in, your reg-config file should look like:**

```json
{
  "core": {
    "workingDir": ".reg",
    "actualDir": "directory_contains_actual_images",
    "threshold": 0,
    "thresholdRate": 0.02,
    "ximgdiff": {
      "invocationType": "client"
    },
    "addIgnore": false
  },
  "plugins": {
    "reg-publish-s3-plugin": {
      "bucketName": "visualregression"
    },
    "reg-simple-keygen-plugin": {
      "expectedKey": "newFeature-iPhoneX3",
      "actualKey": "newFeature-iPhoneX4"
    }
  }
}
```
The **simple-keygen** plug-in allows the user to input a custom expectedKey name (which they already published) and also allows them to choose the name of the actualKey, which allows **the user to have multiple sets of reference images, any of which can be selected to compare agaisnt**