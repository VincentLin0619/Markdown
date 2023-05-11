# Routes
[←回到上一頁](//RubyonRails.md)

### RESTful網址設計

REST是「具象狀態傳輸」（epresentational State Transfer）的縮寫，把每個網址當成資源（Resource）看待，對同一個資源做不同的動作（HTTP Verb）會得到不同的結果，符合REST概念設計的網址，又稱之RESTful Routes。

## 資源Resource

要符合RESTful網址的設計，除了自己一條一條寫，更建議直接使用Rails提供的 resourse 方法

使用 $ rails routes 查看

Prefix routes是什麼？

- 在後面加上_path會產生站內相對應的路徑

    ```ruby
    products + path     = products_path         => /products
    new_product + path  = new_product_path      => /products/new
    edit_product + path = edit_product_path(2)  => /products/2/edit
    ```

- 在後面加上_url會產生完整的路徑，包括主機名稱

    ```ruby
    products + url     = products_url         => http://kaochenlong.com/products
    new_product + url  = new_product_url      => http://kaochenlong.com/products/new
    edit_product + url = edit_product_url(2)  => http://kaochenlong.com/products/2/edit
    ```


**如果不想要自動產生這麼多的路由**

```ruby
Rails.application.routes.draw do
  resources :products, only: [:index, :show]

  # 或是反過來這樣寫也行
  # resources :products, except: [:new, :create, :edit, :update, :destroy]
end
```

單數路由（不產生:id）

```ruby
Rails.application.routes.draw do
  resource :profile
end
```

關聯性的大腸包小腸

```ruby
Rails.application.routes.draw do
  resources :users do
    resources :posts
  end
end
```

大腸包小腸可以寫成 only （或 except ）參數羞成這樣：

```ruby
Rails.application.routes.draw do
  resources :users do
    resources :posts, only: [:index, :new, :create]
  end
  resources :posts, only: [:show, :edit, :update, :destroy]
end
```

### **如果覺得Resource不夠用**

例如想要加上「確認訂單」或是「取消訂單」之類更改訂單狀態的路徑，可使用 `collection`
 或是 `member`
 方法。這兩種用法在使用上有一些差異：

```ruby
# 在後面加參數的方式
GET /orders?type=cancelled

# 或是給它一個獨立的路徑
GET /orders/cancelled
```

使用collection

```ruby
Rails.application.routes.draw do
  resources :orders do
    collection do
      get :cancelled
    end
  end
end
```

把 `collection`包在 `orders` 這個 Resources 裡，這樣的寫法，會產生以下路徑：

```ruby
$ rails routes
          Prefix Verb   URI Pattern                 Controller#Action
cancelled_orders GET    /orders/cancelled(.:format) orders#cancelled
          orders GET    /orders(.:format)           orders#index
                 POST   /orders(.:format)           orders#create
       new_order GET    /orders/new(.:format)       orders#new
      edit_order GET    /orders/:id/edit(.:format)  orders#edit
           order GET    /orders/:id(.:format)       orders#show
                 PATCH  /orders/:id(.:format)       orders#update
                 PUT    /orders/:id(.:format)       orders#update
                 DELETE /orders/:id(.:format)       orders#destroy
```

你可以看到，除了原來的 8 個之外，還多了一個 `/orders/cancelled`的路徑，並且指向 `orders#cancelled`這個 Action。不想用 Block 方式寫，也可這樣寫，效果是一樣的：

```ruby
Rails.application.routes.draw do
  resources :orders do
    get :cancelled, on: :collection
  end
end
```

API-Router

```ruby
## 前詞綴為 api/v1 使用 namespace 包起來

namespace :api do
	namespace :v1 do
		scope :user do
			get '/', to: 'user#index'
			get '/:id', to: 'user#show'
			post ':id/update', to: 'user#update'
			delete ':id', to: 'user#destroy

'
		end
		scope :auth do

		end
	end
end
```
