# 物件的型別
[←回上頁](../Ruby-base.md)

----------------------------------------------------------------
[TOC]
----------------------------------------------------------------
# Number

在Ruby裡所有東西都是物件，即使是init也只是Integer類別的實例化（instance）

- 浮點數不是精準的
    - 若要完全精準需調用BigDecimal做轉換

        ```ruby
        require 'bigdecimal'
        puts BigDecimal("4.51212") == BigDecimal("3.51212") + BigDecimal("1")    # => true
        ```

    - 取餘數方法

        ```ruby
        puts 20.odd? #=>fales
        puts 20.even? #=>true
        ```

    - 四捨五入取第x

        ```ruby
        puts 3.333.round      # => 3
        puts 3.834.round      # => 4

        puts 3.333.round(2)   # => 3.33
        puts 4.518.round(2)   # => 4.52
        ```


# String

- 使用方法

    ```ruby
    name = new String('Hello');

    name = "hello"
    name = 'hello'
    ```

- 字串安插

    ```ruby
    puts "你好，我是 " + name + " 我今年 " + age + " 歲"
    # => 字串跟數字無法直接串接，會出現 TypeErro

    # 需要先把 age 轉換成字串（使用 to_s 方法）
    puts "你好，我是 " + name + " 我今年 " + age.to_s + " 歲"
    # => 印出「你好，我是 eddie 我今年 18 歲」

    ####或是用模板語法######

    name = "eddie"
    age = 18

    puts "你好，我是 #{name}，我今年 #{age} 歲"
    # 必須使用單引號
    # => 印出「你好，我是 eddie，我今年 18 歲」
    ```

- 引號裡的引號

    ```ruby
    puts "我說\"雙引號需要使用反斜線來處理!\""
    # => 印出「我說"雙引號需要使用反斜線來處理!"」

    name = "紅寶石"

    puts %Q(你好，#{name})                   # 跟雙引號一樣，可以使用字串安插
    # => 印出「你好，紅寶石」

    puts %Q(你好，紅寶石"'"'"'"'"''"'"'")    # 要放幾個引號都可以
    # => 印出「你好，紅寶石"'"'"'"'"''"'"'"」

    puts %q(你好，#{name})                   # 跟單引號一樣，不會處理字串安插
    # => 印出「你好，#{name}」
    ```

- 字串當陣列

    ```ruby
    name = "This is a book"
    title = "紅寶石鑑定商"

    puts name[0]        # => T
    puts title[1]       # => 寶

    title[0..1] = "鑽"
    puts title          # => 鑽石鑑定商
    ```

- 字串計算

    ```ruby

    puts "hello, ruby".size        # => 11

    ##非英文
    puts "五倍紅寶石".size         # => 5
    puts "ありがとう".size         # => 5
    puts "五倍紅寶石".bytesize     # => 15
    puts "ありがとう".bytesize     # => 15
    ##bytesize會算出實際byte數

    words = "Ut in aliquam mauris. Donec dolor quam, sagittis id efficitur vel, convallis vitae tortor"
    puts words.split.count    # => 14
    ##去掉空白在計算
    ```

- 檢查是否為空字串的方式

    ```ruby
    ##常用寫法

    if name == ''
      # ...
    else
      # ...
    end

    ##empty?
    puts "hello".empty?    # => false
    puts " ".empty?        # => 裡面有一個空白字元，所以得到 false
    puts "".empty?         # => true
    ```

- 大小寫轉換

    ```ruby
    ##字串大小寫轉換

    puts "hello, ruby".upcase     # => HELLO, RUBY
    puts "HELLO, RUBY".downcase   # => hello, ruby
    puts "Hello, Ruby".swapcase   # => hELLO, rUBY

    ##字首轉換
    puts "eddie".capitalize     # => Eddie
    ```

- 計算某字母出現幾次

    ```ruby
    words = "Lorem Ipsum Dolor Sit Amet, Consectetur Adipiscing Elit."

    puts words.count("i")    # => 算出有幾個 i，共有 5 個
    puts words.count("A-Z")  # => 算出所有大寫字母，共有 8 個
    puts words.count("a-z")  # => 算出所有小寫字母，共有 39 個
    ```

- star_with?, end_with?

    檢查字串是否為XX開頭或結尾

    ```ruby
    puts "Hello, Ruby".start_with?("H")  # => true
    puts "Hello, Ruby".start_with?("h")  # => false
    puts "Hello, Ruby".end_with?("y")    # => true
    ```

- 替換字串（ sub, gsub）

# Array

```ruby
#用 for && each 從陣列取值

the_content = [1, 2, 3, 4, 5]
fruits = ['Apple', 'Banana' , 'Orange']

for number in the_content
	puts "This is count #{number}"
end

fruits.each do |fruit|
	puts "A fruit of type: #{fruit}"
end

#將值塞到陣列

elements = []

for i in (0..5)
	elements.push(i)
end
```

# Range

兩個點：(1..5) ⇒ 1~5

三個點：(1...5) ⇒ 1~4 排除 5

字串也行：(a..z) ⇒ [’a’, ‘b’, ‘c’, ‘d’ .... ‘z’]

轉為陣列方式：(1..5).to_a == (*1..5)

```ruby
#可以不需要轉換為陣列就用操作裡面的元素

###判斷1~100的基數為哪些並puts出來
(1..100).each do |i|
    if(i.odd?)
        puts i
    end
end
```

# Hash

Hash是可以通過任何東西找到元素，不只是數字。並且可以讓一個物件和別的東西做關聯（不管型態為何）

```ruby
stuff = { :name => 'Rob', :age => 18, :height => 5*2+10 }

#使用字串增加物件的元素

stuff[:city] = 'Taiwan'   => puts stuff[:city] #印出 Taiwan

##刪除元素 delete

stuff.delete(:city) #即刪除city這個元素

```

# Symbol

是一種「帶有名字的物件」，為symbol類的實體，描述某個狀態

```ruby
class Order
	attr_reader :status

	def initialize(items, status = pending)
		@items = items
		@status = status
	end

	def compete
		@status = :compete
	end
end

order = Order.new(["item A", "item B", "item C"])

if order.status == :pending
	puts "order is pending"
end
```
