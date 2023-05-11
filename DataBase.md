# DB

Devise 中有三個 action 可以允許設定的欄位通過傳到 model，分別是：

- sign_in ( Devise::SessionsController#create )只允許 authentication keys 通過(ex: email、account)
- sign_up ( Devise::RegistrationsController#create )只允許 authentication keys、password、password_confirmation
- account_update ( Devise::RegistrationsController#update )只允許 authentication keys、password、password_confirmation、 current_password