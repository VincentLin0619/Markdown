# Method,Block
[←回上頁](../Ruby-base.md)

----------------------------------------------------------------
[TOC]
----------------------------------------------------------------

# 程式碼區塊（Block）

- ****Block 不是物件****

    作用域？？

    {}

    do ... end

- ****Block 不是參數****

    Block 通常得像寄生蟲一樣依附或寄生在其它的方法或物件（或是使用某些類別把它物件化），但它不是參數

- ****如何執行 Block 的內容？****

    > 某 Block：「嘿，say_hello_to 方法你好，我要掛在你身上囉」
    >

    > say_hello_to ：「隨便啊，你要掛就讓你掛，但要不要讓你執行是我決定的！」
    >
- **yield**

    將控制權先交給block(回呼函示？？)

    ```ruby
    def say_hello
      puts "開始"
      yield          # 把控制權暫時讓給 Block
      puts "結束"
    end

    say_hello {
      puts "這裡是 Block"
    }
    ```

- **傳參數給block**

    ```ruby
    5.times do |i|
      puts i   # 這個變數 i 只有在 Block 裡有效，會依序印出數字 0 到 4
    end

    puts i     # 離開 Block 之後就失效，出現找不到變數的錯誤（NameError）
    ```

    在使用yield方法把控制權轉讓給block時，順便把值帶給block

    ```ruby
    def say_hello
      puts "開始"
      yield 123   # 把控制權暫時讓給 Block，並且傳數字 123 給 Block
      puts "結束"
    end

    say_hello { |x| # 這個 x 是來自 yield 方法
      puts "這裡是 Block，我收到了 #{x}"
    }
    ```

- ****用 return 回傳 Block 的結果？****

    Block 的最後一行執行結果自動會變成 Block 的回傳值，這裡並不是省略了 return，而是不能使用 return 回傳結果。所以如果你在上面那個例子，在 Block 裡試圖用 `return`
     回傳結果，像這樣：

    ```ruby
    pick([*1..10]) { |x| return x % 2 == 0 }
    ```

- **`5.times { ... }` 很好用，但你能自己土砲一個類似的方法嗎？**

    ```ruby
    def my_times(n)
      i = 0
      while n > i
        i += 1
        yield i
      end
    end

    my_times(5) { |num|
      puts "hello, #{num}xRuby"
    }

    # 得到結果
    # hello, 1xRuby
    # hello, 2xRuby
    # hello, 3xRuby
    # hello, 4xRuby
    # hello, 5xRuby
    ```

- ****大括號跟 do … end 的差別****

    ```ruby
    # 使用 do .. end 寫法
    5.times do
      puts "哈囉，世界"
    end

    # 使用大括號寫法
    5.times {
      puts "哈囉，世界"
    }
    # 使用大括號一行寫法
    5.times { puts "哈囉，世界" }
    ```

- ****把 Block 物件化****

    ```ruby
    greeting = Proc.new { puts "哈囉，世界" }   # 使用 Proc 類別可把 Block 物件化
    greeting.call  # 印出 "哈囉，世界"

    #如果要帶參數
    say_hello_to = Proc.new { |name| puts "你好，#{name}"}
    say_hello_to.call("尼特羅會長")
    ```

- ****Proc 呼叫方式****

    ```ruby
    say_hello_to.call("尼特羅會長")    # 使用 call 方法
    say_hello_to.("尼特羅會長")        # 使用小括號（注意，有多一個小數點）
    say_hello_to["尼特羅會長"]         # 使用中括號
    say_hello_to === "尼特羅會長"      # 使用三個等號
    say_hello_to.yield "尼特羅會長"    # 使用 yield 方法
    ```


# 方法（Method）

- ****定義方法****

    使用def

    ```ruby
    def say_hello(name)
    	puts "hello! #{name}"
    end
    ```

- ****呼叫方法****

    ```ruby
    say_hello('Bob')  #=> Hello! Bob

    #可視情況省略小
    say_hello "Bob"
    ```

- ****關於方法的冷知識****

    定義方法時，Ruby會產生一筆新的Symbol存於記憶體，要調用此方法的話可以使用send，把該筆symbol調用出來

    ```ruby
    send(:say_hello, "Bob")
    ```

- ****參數預設值****

    ```ruby
    def say_hello(name = 'vint')
    	puts "hello! #{name}"
    end

    say_hello()       #=> hello! vint
    send(:say_hello)  #=> hello! vint
    ```

- ****方法的回傳值****

    ****puts 不是 return，也沒有回傳值****

    ```ruby
    def bmi_calculator(height, weight)
        puts weight / (height / 100.0) ** 2  ####整數除法要注意####
    end

    bmi_calculator(180, 70)
    ```

- ****問號跟驚嘆號也是方法的一部份****

    通常會使用問號，期望這個方法是會回傳一個布林值，這只是一種慣例

    ```ruby
    puts "".empty?                       # => true
    puts [1, 2, 3, 4, 5].include?(3)     # => true
    puts "Ruby".start_with?("Ru")        # => true
    ```

    驚嘆號表示方法可能會有「副作用」或是「驚喜」

    ```ruby
    #example => reverse會產生一個新的陣列，不會影響原本的陣列
    original_list = [1, 2, 3, 4, 5]
    reversed_list = original_list.reverse

    p reversed_list   # => [5, 4, 3, 2, 1]
    p original_list   # => [1, 2, 3, 4, 5]

    #but => reverse加上驚嘆號後，除了回傳新陣列以外也會改變原有陣列
    original_list = [1, 2, 3, 4, 5]
    reversed_list = original_list.reverse!

    p reversed_list   # => [5, 4, 3, 2, 1]
    p original_list   # => [5, 4, 3, 2, 1]
    ```
