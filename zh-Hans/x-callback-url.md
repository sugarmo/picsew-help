# x-callback-url 文档

Picsew 实现了 [x-callback-url](http://x-callback-url.com/) 协议，这是一种通用的 URL Scheme 协议。它能让你在不同的 App 之间通信，[Workflow](https://workflow.is/)、[Launch Center Pro](https://contrast.co/launch-center-pro/) 等 App 都支持了 x-callback-url，因此 Picsew 也支持与他们协作。

Picsew 的 x-callback-url 格式为：

```
picsew://x-callback-url/[动作]?[动作参数]&[x-callback 参数]
```

## 动作

### /scroll

使用指定的图片进行长截图拼接

### /vert

使用指定的图片进行竖向拼接。

### /hori

使用指定的图片进行横向拼接。

## 动作参数

- **in** *（必选）*指定如何获取输入图片，可选值为 `paste`、`latest` 和 `recent`。当为 `paste` 时，从剪贴板粘贴图片。当为 `latest` 时，获取设备相册的最后 N 张图片，N 由另一个参数 `count` 决定。当为 `recent` 时，自动检测最近的截图。

- **count** 当 `in=latest` 时，为必选参数，指定要获取的图片的数量。
    
- **out** *（必选）*指定如何输出拼接结果，可选值为 `copy`、`save` 和 `save_copy`。当为 `copy` 时，复制结果到剪贴板。当为 `save` 时，保存结果到设备相册。当为 `save_copy` 时，保存并复制结果。

- **watermark** *（可选）*指定是否加上水印，可选值为 `single` 和 `repeat`，默认不加上水印。水印文字和位置都使用 App 内的默认水印设置，如果未设置妥当，则最终结果也不会有水印。当为 `single` 时，在拼图的默认位置添加一个水印。当为 `repeat` 时，在每张图的默认位置都添加一个水印。

- **mockup** *（可选）*指定是否加上设备外壳，可选值为 `white` 和 `black`，默认不加上外壳。外壳自动选取，选取逻辑跟 App 内保持一致。如果拼图比例不够修长，则最终结果依然不会加上外壳。当为 `white` 时，默认选取金色（iPhone X 则为银色），当为 `black` 时，默认选取灰色。

- **clean_status** *（可选）*指定是否需要清理状态栏，当为 `yes` 时，自动清理状态栏。默认不清理。

- **delete_source** *（可选）*指定是否需要删除来源图片，当为 `yes` 时，删除来源图片。默认不删除。


## 返回参数

### X-SUCCESS

- **local_id** *（可能为空）*当选择保存结果到设备相册时，该参数为对应的 `PHAsset` 的 `localIdentifier`。


## 例子

- 使用最近的截图进行长截图拼接，自动清理状态栏，并且加上白色设备外壳，把结果保存到设备相册，然后删除来源图片。

```
picsew://x-callback-url/scroll?in=recent&out=save&clean_status=yes&mockup=white&delete_source=yes
```

- 使用设备相册的最后3张图进行竖向拼接，每张图片都加上默认水印，把结果复制到剪贴板。

```
picsew://x-callback-url/vert?in=latest&count=3&out=copy&watermark=repeat
```

## Workflow 例子

**[制作长截图](https://workflow.is/workflows/e9b64bc79d854bb0a9f9531d6cab5bdd)** *挂件*

这个工作流会让你选择最新截图的数量，调起 Picsew 来自动拼接，保存图片到相册，然回到 Workflow，查看结果，删除来源图片。

**[最近长截图](https://workflow.is/workflows/b3084df208c34b74877471bddad84576)** *挂件*

这个工作流会自动检测最近的截图，调起 Picsew 来自动拼接，并且根据你的选择加上设备外壳或者清理状态栏，保存图片到相册，删除来源图片，然回到 Workflow，查看结果。

**[创建长截图](https://workflow.is/workflows/a9c746a2306e400c914d274b5d0998bd)** *动作*

这个工作流会根据你选择的截图，调起 Picsew 来自动拼接，并且根据你的选择加上设备外壳或者清理状态栏，保存图片到相册，然回到 Workflow，查看结果，删除来源图片。
