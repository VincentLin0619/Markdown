# 佈署到 Render 的一些設定

## 設定database.ymal  環境變數。

## 設定Puma.rb這兩行註解掉
![](https://hackmd.io/_uploads/HyzXqMcN2.png)

## 設定production.rb
![](https://hackmd.io/_uploads/ByRScGcVn.png)

## 建立一個render-build.sh來自動跑build script
![code2](https://hackmd.io/_uploads/Sy4aYM9Eh.png)


# render.yaml的配置

在根目錄底下
![code3](https://hackmd.io/_uploads/Sk_9tGcEh.png)

要跑這行

```bash
bundle lock --add-platform x86_64-linux
```

在Render使用blueprint創建專案的時候把config/master.key的密鑰貼到環境變數上。
