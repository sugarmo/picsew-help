# x-callback-url Document

Picsew implements the [x-callback-url](http://x-callback-url.com/) protocol, a generic URL Scheme protocol. It allows you to communicate between different apps. [Workflow](https://workflow.is/), [Launch Center Pro](https://contrast.co/launch-center-pro/) and other apps support x-callback-url, so Picsew also supports working with them.

Picsew's x-callback-url format is:

```
picsew://x-callback-url/[action]?[action parameters]&[x-callback parameters]
```

## Action

### /scroll

Use the specified images for Scrollshot Stitching.

### /vert

Use the specified images for Vertical Stitching.

### /hori

Use the specified images for Horizontal Stitching.

## Action Parameters

- **in** *(Required)* Specifies how to get the input image. The allowed values ​​are `paste`, `latest`, and `recent`. When `paste`, get the images from the clipboard. When `latest`, get the latest N images of the photo album, and N is determined by another parameter `count`. When `recent`, the most recent screenshots are automatically detected.

- **count** When `in=latest`, this is a required parameter, specify the number of images to get.
    
- **out** *(Required)* Specifies how to output the stitching result. The allowed values ​​are `copy` and `save`. When `copy`, copy the result to the clipboard. When `save`, save the result to the photo album.

- **watermark** *(optional)* Specifies whether to add watermark. The allowed values ​​are `single` and `repeat`. By default, no watermark is added. Both watermark text and location use the default watermark setting in the App. If default setting not configure properly, the final result will not have a watermark. When `single`, add a single watermark to the default position of the result image. When `repeat`, a watermark is added to the default position of each image.

- **mockup** *(optional)* Specifies whether to add the device mockup. The allowed values ​​are `white` and `black`. The mockup is not added by default. The mockup is automatically selected. The selection logic is consistent with the App. If the proportion of result image is not long enough, the final result will still not add a mockup. When `white`, gold color is selected by default (iPhone X is silver color), and when `black` is selected, gray color is selected by default.

- **clean_status** *(optional)* Specifies whether the status bar needs to be cleared. When `yes`, the status bar is automatically cleared. The default is not clear.

- **delete_source** *(optional)* Specifies whether the source images needs to be deleted. When `yes`, the source images will be deleted. It is not deleted by default.

## Return Parameters

### X-SUCCESS

- **local_id** *(may be missing)* When you choose to save the result to the photo album, the value of this parameter is the `localIdentifier` of the corresponding `PHAsset`.

## Examples

- Use the most recent screenshots for scrollshot stitching, automatically clear the status bar, and add a white device mockup, save the results to the photo album, and then delete the source images.

```
picsew://x-callback-url/scroll?in=recent&out=save&clean_status=yes&mockup=white&delete_source=yes
```

- Vertical stitching using the latest 3 images of the photo album, add a default watermark to each image, copy the result to the clipboard.

```
picsew://x-callback-url/vert?in=latest&count=3&out=copy&mockup=white
```
