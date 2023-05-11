# Rspec
[TOC]


---

`factory_bot_rails` 則是一個 Ruby on Rails 的資料產生器，用來產生物件，讓開發者可以在測試和開發中使用。相比於 `faker`，`factory_bot_rails` 提供了更多的自定義選項和更豐富的功能，可以讓開發者快速地建立模型物件，減少開發時間，同時也能夠保持測試數據的一致性和可讀性。

## faker

faker 是一個 Ruby 語言的假資料產生器，用來在測試或開發時，快速產生一些假資料填充到資料庫中，以測試應用程式的功能是否正確，同時也能夠加快開發速度。

## capybara

`Capybara` 是一個 Ruby 語言的 Web 應用程式測試框架，用來模擬使用者在瀏覽器上的操作，例如點擊連結、填寫表單、提交表單等等，以測試 Web 應用程式的功能是否正確。`Capybara` 提供了一套 DSL（Domain-Specific Language）語言，讓使用者可以用自然的方式撰寫測試案例，並支援多種不同的驅動程式，例如 `Selenium`、`Rack-Test`、`Poltergeist` 等，讓開發者可以依照需求選擇最合適的驅動程式。Capybara 能夠讓開發者寫出簡潔、易讀的測試案例，同時也能夠確保 Web 應用程式的正確運作，提高軟體品質。

## database_cleaner-active_record

**`database_cleaner-active_record`**
 是一個 Ruby Gem，用於在 Ruby on Rails 應用中協助清理測試資料庫的數據。測試中經常需要創建和刪除數據，
 可以在測試開始前清空資料庫表，或在測試結束後刪除創建的數據。它可以與不同的數據庫策略一起使用，例如transaction、truncation、deletion 等，以清理資料表中的數據。

**`truncation`** 和 **`transaction`**

`truncation` 是指使用 `TRUNCATE SQL` 語句將表格清空，這是一種非常快速且有效的清空表格的方法。這種方式比 `DELETE SQL` 語句快得多，因為它不會記錄日志和觸發 `DELETE` 觸發器。

`transaction` 是指使用數據庫事務將測試期間所做的更改回滾。在測試過程中，所有更改都會在事務中完成，而在測試完成後，這些更改將被回滾，從而保持數據庫的原始狀態。這種方法的好處是，它可以確保測試不會影響到其他的測試或應用程序，同時也確保了每個測試的幹凈狀態。缺點是在測試中使用了大量數據時，事務的回滾操作可能會變得很慢。

```ruby
require 'rails_helper'

RSpec.feature "User index", type: :feature do
  before(:all) do
    DatabaseCleaner.strategy = :truncation #改為清除模式而不是回滾
  end

  before(:each) do
    DatabaseCleaner.start
  end

  before(:each) do
    DatabaseCleaner.clean
  end

  # 清除之後在創建使用者
  before(:each) do
    create_list(:user, 5)
  end

  it 'lists user in json' do
    visit api_v1_path(format: :json)
    json = JSON.parse(page.body)
    pp json
    expect(json.length).to eq(5)
    expect(json[0]['nick_name']).to eq('test1')

  end

end
```



# Hooks

## 作用順序與優先程度


>**`:each`**：這個參數表示鉤子函數在每個測試用例之前/之後都會運行。也就是說，在每個測試用例運行之前，都會先執行一次 **`before(:each)`**，然後在測試用例運行之後再執行一次 **`after(:each)`**。這是默認的行為，如果沒有指定參數，就會使用 **`:each`**。
>**`:suite`**：這個參數表示鉤子函數在整個測試套件之前/之後運行，也就是說，在所有測試用例運行之前，會先執行一次 **`before(:suite)`**，然後在所有測試用例運行之後，再執行一次 **`after(:suite)`**。這個參數一般用於在測試套件啟動或結束時執行一些全局的準備或清理工作。
>**`:all`**：這個參數表示鉤子函數在整個測試運行過程中只運行一次。也就是說，在所有測試用例運行之前，只會執行一次 **`before(:all)`**，然後在所有測試用例運行之後，只會執行一次 **`after(:all)`**。這個參數一般用於在整個測試過程中只需要執行一次的準備或清理工作，比如創建/刪除數據庫等。

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

## Before & After Hooks

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

**`after`** Hooks 則會在每個測試運行之後運行，也可以有多個，且可以有多種形式：

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

## Matcher

除了在 RSpec 中常用的**`eq`**匹配器外，還有許多其他匹配器可以用於進行不同類型的斷言。以下是一些常用的匹配器：

1. **`be`**：用於檢查一個值是否為真或假，或是否等於另一個值。
    
    ```ruby
    expect(result).to be true
    expect(result).to be_falsey
    expect(result).to be_nil
    ```
    
2. **`include`**：用於檢查一個集合是否包含特定的元素。
    
    ```ruby
    expect(array).to include(1)
    expect(string).to include("hello")
    ```
    
3. **`match`**：用於檢查一個值是否符合正則表達式。例如：
    
    ```ruby
    expect(string).to match(/hello/)
    ```
    

4. **`raise_error`**：用於檢查一個代碼塊是否引發了特定的異常。
    
    ```ruby
    expect { code_that_raises_error }.to raise_error(RuntimeError)
    ```
    
# 起手式

describe 方法用於定義測試組，而 it 方法用於定義單獨的測試用例。describe 和 it 方法的結合使用可以幫助我們撰寫更清晰、更易於維護的單元測試代碼。

<details>
    <summary>describe method</summary>
    describe 方法用於定義一個測試組（test suite），通常會包含多個 it 塊。describe 方法可以嵌套使用，因此可以將多個測試組組合起來，形成一個更大的測試組。
</details>

<details>
    <summary>it method</summary>
    it 方法用於定義一個單獨的測試用例（test case），即一個具體的測試任務。每個 it 塊都應該測試一個特定的功能或行為，並且應該是獨立的，不依賴其他測試。
</details>

```ruby
describe Calculator do
  describe "#add" do
    it "adds two numbers together" do
      calculator = Calculator.new
      expect(calculator.add(2, 3)).to eq(5)
    end
  end

  describe "#subtract" do
    it "subtracts one number from another" do
      calculator = Calculator.new
      expect(calculator.subtract(5, 3)).to eq(2)
    end
  end

  describe "#multiply" do
    it "multiplies two numbers together" do
      calculator = Calculator.new
      expect(calculator.multiply(2, 3)).to eq(6)
    end
  end

  describe "#divide" do
    it "divides one number by another" do
      calculator = Calculator.new
      expect(calculator.divide(6, 3)).to eq(2)
    end
  end
end
```


# 測試的規範層級

```ruby
RSpec.describe Calculator do
  describe '#add' do
    context 'when adding positive numbers' do
      it 'returns the sum of two positive numbers' do
        calculator = Calculator.new
        result = calculator.add(2, 3)
        expect(result).to eq(5)
      end
    end

    context 'when adding negative numbers' do
      it 'returns the sum of two negative numbers' do
        calculator = Calculator.new
        result = calculator.add(-2, -3)
        expect(result).to eq(-5)
      end
    end
  end
end
```

上述範例 RSpec 測試套件來測試一個 **`Calculator`** 類別的 **`add`** 方法。在 **`RSpec.describe`** 命令中，我們定義了一個測試套件，名稱為 **`Calculator`**。在套件內部，我們使用了 **`describe`** 命令來定義一個測試上下文，名稱為 **`#add`**。在 **`#add`** 上下文中，我們使用了兩個 **`context`** 命令，分別代表兩種不同的情境：當加入正數時，以及當加入負數時。

在每個 **`context`** 內部，我們使用了 **`it`** 命令來定義具體的測試案例。例如，在第一個 **`context`** 中，我們定義了一個測試案例，它測試在加入兩個正數時 **`add`** 方法是否會回傳正確的結果。在這個測試案例中，我們創建了一個 **`Calculator`** 物件，並呼叫 **`add`** 方法來加入兩個正數。最後，我們使用 **`expect`** 命令來斷言 **`add`** 方法的回傳值是否等於 5。

透過這樣的層級關係，我們可以組織測試案例，讓它們更具可讀性和可維護性。測試套件提供了一個最高層級的組織，它可以包含多個測試上下文。每個測試上下文代表一個特定的方法或行為，它可以包含多個測試案例。每個測試案例都代表一個具體的測試項目，它們可以包含多個斷言，用於檢查被測試的程式碼是否正確。


# 斷言

```ruby
expect(actual_value).to eq(expected_value)
// 這裡我預期 actual_value 等於 expected_value
```

expect方法適用於進行斷言時檢查實際值是否等於預期值。

eq 為匹配器

以上述的方法來說：

```ruby
calculator = Calculator.new
expect(calculator.add(2, 3)).to eq(5)

#實例化 Calculator
#斷言說我預期add(2,3) 會等於5

#如果這個斷言通過,就會噴綠
#如果沒通過,會顯示類似以下訊息
#說明你預期值為5,但實際值得到6
expected: 5
     got: 6
```