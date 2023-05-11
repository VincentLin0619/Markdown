# Matcher

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