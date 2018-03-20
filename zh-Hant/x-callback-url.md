# x-callback-url 文檔

Picsew 實現了 [x-callback-url](http://x-callback-url.com/) 協議，這是一種通用的 URL Scheme 協議。它能讓你在不同的 App 之間通信，[Workflow](https://workflow.is/)、[Launch Center Pro](https://contrast.co/launch-center-pro/) 等 App 都支持了 x-callback-url，因此 Picsew 也支持與他們協作。

Picsew 的 x-callback-url 格式為：

```
picsew://x-callback-url/[動作]?[動作參數]&[x-callback 參數]
```

## 動作

### /scroll

使用指定的圖片進行長截圖拼接

### /vert

使用指定的圖片進行豎向拼接。

### /hori

使用指定的圖片進行橫向拼接。

## 動作參數

- **in** *（必選）*指定如何獲取輸入圖片，可選值為 `paste`、`latest` 和 `recent`。當為 `paste` 時，從剪貼板粘貼圖片。當為 `latest` 時，獲取設備相冊的最後 N 張圖片，N 由另一個參數 `count` 決定。當為 `recent` 時，自動檢測最近的截圖。

- **count** 當 `in=latest` 時，為必選參數，指定要獲取的圖片的數量。
    
- **out** *（必選）*指定如何輸出拼接結果，可選值為 `copy`、`save` 和 `save_copy`。當為 `copy` 時，復制結果到剪貼板。當為 `save` 時，保存結果到設備相冊。當為 `save_copy` 時，保存並復制結果。

- **watermark** *（可選）*指定是否加上水印，可選值為 `single` 和 `repeat`，默認不加上水印。水印文字和位置都使用 App 內的默認水印設置，如果未設置妥當，則最終結果也不會有水印。當為 `single` 時，在拼圖的默認位置添加一個水印。當為 `repeat` 時，在每張圖的默認位置都添加一個水印。

- **mockup** *（可選）*指定是否加上設備外殼，可選值為 `white` 和 `black`，默認不加上外殼。外殼自動選取，選取邏輯跟 App 內保持一致。如果拼圖比例不夠修長，則最終結果依然不會加上外殼。當為 `white` 時，默認選取金色（iPhone X 則為銀色），當為 `black` 時，默認選取灰色。

- **clean_status** *（可選）*指定是否需要清理狀態欄，當為 `yes` 時，自動清理狀態欄。默認不清理。

- **delete_source** *（可選）*指定是否需要刪除來源圖片，當為 `yes` 時，刪除來源圖片。默認不刪除。


## 返回參數

### X-SUCCESS

- **local_id** *（可能為空）*當選擇保存結果到設備相冊時，該參數為對應的 `PHAsset` 的 `localIdentifier`。


## 例子

- 使用最近的截圖進行長截圖拼接，自動清理狀態欄，並且加上白色設備外殼，把結果保存到設備相冊，然後刪除來源圖片。

```
picsew://x-callback-url/scroll?in=recent&out=save&clean_status=yes&mockup=white&delete_source=yes
```

- 使用設備相冊的最後3張圖進行豎向拼接，每張圖片都加上默認水印，把結果復制到剪貼板。

```
picsew://x-callback-url/vert?in=latest&count=3&out=copy&watermark=repeat
```

## Workflow 例子

**[制作長截圖](https://workflow.is/workflows/e9b64bc79d854bb0a9f9531d6cab5bdd)** *掛件*

這個工作流會讓你選擇最新截圖的數量，調起 Picsew 來自動拼接，保存圖片到相冊，然回到 Workflow，查看結果，刪除來源圖片。

**[最近長截圖](https://workflow.is/workflows/b3084df208c34b74877471bddad84576)** *掛件*

這個工作流會自動檢測最近的截圖，調起 Picsew 來自動拼接，並且根據你的選擇加上設備外殼或者清理狀態欄，保存圖片到相冊，刪除來源圖片，然回到 Workflow，查看結果。

**[創建長截圖](https://workflow.is/workflows/a9c746a2306e400c914d274b5d0998bd)** *動作*

這個工作流會根據你選擇的截圖，調起 Picsew 來自動拼接，並且根據你的選擇加上設備外殼或者清理狀態欄，保存圖片到相冊，然回到 Workflow，查看結果，刪除來源圖片。
