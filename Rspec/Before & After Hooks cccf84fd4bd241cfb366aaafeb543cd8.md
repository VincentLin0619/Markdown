# Before & After Hooks

在 RSpec 中，可以使用 **`before`** 和 **`after`** Hooks 在執行測試前或後執行某些代碼。這些 Hooks 可以用來準備測試環境、清理測試數據或共享代碼等。

**`before`** Hooks 會在每個測試運行之前運行，它可以有多個，且可以有多種形式：

```ruby
RSpec.describe Person do
  before do
    # 在每個測試運行之前都會運行這個代碼
    @person = Person.new
  end

  before(:context) do
    # 在所有測試運行之前只運行一次這個代碼
    Database.connect
  end

  before(:each) do
    # 在每個測試運行之前都會運行這個代碼
    @person = Person.new
  end

  before(:example) do |example|
    # 在每個測試運行之前都會運行這個代碼
    # 這個代碼可以訪問當前測試的 example 對象
    @person = Person.new
  end
end
```

**`after`**Hooks 則會在每個測試運行之後運行，也可以有多個，且可以有多種形式：

```ruby
RSpec.describe Person do
  after do
    # 在每個測試運行之後都會運行這個代碼
    @person.destroy
  end

  after(:context) do
    # 在所有測試運行之後只運行一次這個代碼
    Database.disconnect
  end

  after(:each) do
    # 在每個測試運行之後都會運行這個代碼
    @person.destroy
  end

  after(:example) do |example|
    # 在每個測試運行之後都會運行這個代碼
    # 這個代碼可以訪問當前測試的 example 對象
    @person.destroy
  end
end
```

<aside>
💡 需要注意的是，**`before`** 和 **`after`** Hooks 的執行順序是有規則的。RSpec 遵循了一個稱為 "Four-Phase Test" 的模式，其中 **`before(:context)`**和 **`after(:context)`** Hooks 只運行一次，而 **`before(:each)`** 和 **`after(:each)`** Hooks 會在每個測試運行之前和之後分別運行一次。此外，**`before(:example)`** 和 **`after(:example)`** Hooks 可以訪問當前測試的 example 對象，因此可以在其中添加特定於當前測試的代碼。

</aside>