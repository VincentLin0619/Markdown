
# Devise

[TOC]

# 安裝

```
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


創建User

```ruby
rails g devise:User
```

如果要客製Controllers && Views

```ruby
rails g devise:controllers user
rails g devise:views user
```



登入或註冊後會被導向/users的問題

```ruby
Rails.application.routes.draw do

	# 繞過Devise 預設的的route
  devise_for :users, skip: [:registrations, :sessions]

  # 處理錯誤會被導到 /users 頁面問題
  devise_scope :user do
    get "users/sign_in", to: "users/sessions#new", as: :new_user_session
    post "users/sign_in", to: "users/sessions#create", as: :user_session
    get "users/sign_up", to: "users/registrations#new", as: :new_user_registration
    post "users/sign_up", to: "users/registrations#create", as: :user_registration
  end
end
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
如果在User資料表新增了欄位，必須要加入強參數（**Strong Parameters**），在concerns底下新增devise_whitelist.rb

```ruby
# app/concerns/devise_whitelist.rb

class DeviseWhitelist < ActionController::Base
  extend ActiveSupport::Concenr
  included do
    before_action :configure_permitted_parameters, if: :devise_controller?
  end

  def configure_permitted_parameters
    #註冊頁面
    devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
    #修改註冊資料
    devise_parameter_sanitizer.permit(:account_update, keys: [:name])
  end
end

# 在application_controller.rb引入

module I18nInitial
  extend ActiveSupport::Concern
  included do
    before_action :set_locale
    around_action :switch_locale
  end
  # 設定語系
  def set_locale
    # 可以將 ["en", "zh-TW"] 設定為 VALID_LANG 放到 config/environment.rb 中
    if params[:locale] && I18n.available_locales.include?(params[:locale].to_sym)
      session[:locale] = params[:locale]
    end
    I18n.locale = params[:locale] || I18n.default_locale
  end

  # 語系切換
  def switch_locale(&action)
    locale = params[:locale] || I18n.default_locale
    I18n.with_locale(locale, &action)
  end

  #使用者選擇語系後可持續在其他頁面使用該語系
  def default_url_options(options = {})
    { :locale => I18n.locale }.merge options
  end

  # 從子網域取得locale code
  def extract_locale_from_tld
    parsed_locale = request.subdomains.first
    I18n.available_locales.map(&:to_s).include?(parsed_locale) ?
      parsed_locale : nil
  end
end
```

# 注意

Devise 中有三個 action 可以允許設定的欄位通過傳到 model，分別是：

- `sign_in` ( Devise::SessionsController#create )只允許 authentication keys 通過(ex: email、account)
- `sign_up` ( Devise::RegistrationsController#create )只允許 authentication keys、password、password_confirmation
- `account_update` ( Devise::RegistrationsController#update )只允許 authentication keys、password、password_confirmation、 current_password