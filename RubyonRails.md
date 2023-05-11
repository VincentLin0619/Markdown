{.no_toc}
# Ruby on Rails
[Ruby-Base](RubyonRails/Ruby-base.md)
[Mailer](RubyonRails/Mailer.md)
[Action_text](RubyonRails/Action_text.md)
[DB](RubyonRails/DB.md)
[Rspec](RubyonRails/Rspec.md)
[一些小操](RubyonRails/%E4%B8%80%E4%BA%9B%E5%B0%8F%E6%93%8D.md)
[Gem](RubyonRails/Gem.md)
[佈署到 Render](RubyonRails/佈署到Render.md)
[API-Route](RubyonRails/API-Route.md)
[ActiveStorage](/RubyonRails/ActiveStorage.md)
[Rails7](/RubyonRails/Rails7.md)
________________________________________________________________
[TOC]
________________________________________________________________



DataBase：PostgreSQL

# 環境建置：

- Ruby-3.0.0
- Rails-6.1.7

# 常用指令：

| 指令 | 簡寫 | 用途 |
| --- | --- | --- |
| rails generate | rails g | 產生各種需要的檔案：scaffold、controller、model等 |
| rails destroy | rails d | 刪除產生器所產生的檔案 |
| rails server | rails s | 啟動伺服器 |
| rails console | rails c | Rails的IRB介面 |
| rails dbconsole | rails db | 使用SQL語法直接操作資料庫 |
| bundle install | bundle | 安裝gem.file的套件 |
| rake test | rake | 執行測試 |
| rails routes
 |  | 列出所有路由 |
| rails routes -grep <keyword> | rails routes -g <keyword> | 搜尋特定路由 (grep) |
| rails routes -c admin/user |  | 搜尋路由中特定 controller |

# 目錄結構(MVC)：

```python
#MVC
├───app
  ├───assets
  │   ├───config
  │   ├───images
  │   └───stylesheets
  ├───channels
  │   └───application_cable
  ├───controllers     #controller所在目錄
  │   └───concerns
  ├───helpers
  ├───javascript
  │   ├───channels
  │   └───packs
  ├───jobs
  ├───mailers
  ├───models           #model所在目錄
  │   └───concerns
  └───views            #view所在目錄
        └───layouts

#Route under config folder
│   application.rb
│   boot.rb
│   cable.yml
│   credentials.yml.enc
│   database.yml
│   environment.rb
│   puma.rb
│   routes.rb          #Route
│   spring.rb
│   storage.yml
│   webpacker.yml
```

# 接收form資料宣告成物件

```python
#example
class BmiController < ApplicationController
  def index
  end

#接收index表單內的值
  def result
    height = params[:body_height].to_f / 100   # 把單位換算成公尺
    weight = params[:body_weight].to_f

    # BMI 計算公式: BMI = 體重（單位：公斤） / 身高平方（單位：公尺）.
    @bmi = (weight / (height * height)).round(2)
  end
end
```


