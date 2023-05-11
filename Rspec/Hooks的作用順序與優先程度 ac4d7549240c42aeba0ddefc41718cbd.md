# Hooks的作用順序與優先程度

1. **`before(:suite)`**：在所有測試套件之前運行一次。
2. **`before(:all)`**：在每個測試套件之前運行一次。
3. **`before(:context)`**：在每個測試上下文之前運行一次。
4. **`before(:example)`**：在每個測試之前運行一次。
5. **`after(:example)`**：在每個測試之後運行一次。
6. **`after(:context)`**：在每個測試上下文之後運行一次。
7. **`after(:all)`**：在每個測試套件之後運行一次。
8. **`after(:suite)`**：在所有測試套件之後運行一次。

優先程度由高到低，從第一個到最後一個。也就是說，在測試套件開始運行之前，RSpec 會先執行 **`before(:suite)`** Hook，然後依次運行所有的測試套件和測試，最後再依次執行 **`after(:suite)`** Hook。

在每個測試套件中，RSpec 會先執行 **`before(:all)`** Hook，然後依次運行所有的測試上下文和測試，最後再執行 **`after(:all)`** Hook。

在每個測試上下文和測試中，RSpec 會先執行 **`before(:context)`** 和 **`before(:example)`** Hooks，然後執行測試代碼，最後再執行 **`after(:example)`** 和 **`after(:context)`** Hooks。注意，在每個測試中，**`before(:context)`** Hook 只會在其上層測試上下文的 **`before(:context)`** Hook 運行之後才運行，而 **`after(:context)`** Hook 只會在其下層測試上下文的 **`after(:context)`** Hook 運行之後才運行。