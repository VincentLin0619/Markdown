# Devise

```
# 安裝
rails g devise:install
```

產生view

```ruby
rails g devise:views User
```

客製化view

```ruby
# 打開config/initializers/devise.rb並打開config.scoped_views

# "users/sessions/new". It's turned off by default because it's slower if you
# are using only default views.
config.scoped_views = true
```

## 新增欄位，必須要設定強參數

在 Devise 中只有三個 action 允許設定的欄位通過傳到 model，分別是：

- `sign_in` ( Devise::SessionsController#create )只允許 authentication keys 通過( ex: email、account)
- `sign_up` ( Devise::RegistrationsController#create )只允許 authentication keys、password、password_confirmation
- `account_update` ( Devise::RegistrationsController#update )只允許 authentication keys、password、password_confirmation、 current_password

```ruby
# 把devise的controller建出來
rails g devise:controllers MODLE

# routes設定要複寫哪些devise內建的controller
devise_for :users, controllers: { session: "users/sessions", registrations: "users/registrations" }
```

```ruby
# 在要更改的Controller裡，ex./registrations_controller.rb
# 依照文檔的註解拿掉，依序修改

class Users::RegistrationsController < Devise::RegistrationsController
  before_action :configure_sign_up_params, only: [:create]
  before_action :configure_account_update_params, only: [:update]

# ...略

def configure_sign_up_params # 讓create通過的參數
    devise_parameter_sanitizer.permit(:sign_up, keys: [:nick_name, :role])
  end

  def configure_account_update_params # 讓更新通過的參數
    devise_parameter_sanitizer.permit(:account_update, keys: [:nick_name, :role])
  end

  def after_sign_up_path_for(resource)
    super(resource)
  end

  def after_inactive_sign_up_path_for(resource)
    super(resource)
  end
end
```

懶人方法，在上層ApplicationController設定，並讓DeviseController去繼承

```ruby
class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?
# 這一行的意思是只有devise所生成的controller、才會在action前，執行configure_permitted_parameters

  protected
# 單行設定
		devise_parameter_sanitizer.permit(:sign_up, keys:[:first_name]
  def configure_permitted_parameters
# 若你的欄位設計成巢狀的，也可以這樣設定！
    devise_parameter_sanitizer.permit(:sign_up, 
      keys: [
        :first_name, :last_name, 
        address_attributes: [
          :country, :state, :city, :area, :postal_code
        ]
      ]
    )
  end
end
```