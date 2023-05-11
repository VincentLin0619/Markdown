# Ruby-base
[←回到上一頁](//RubyonRails.md)
________________________________
[toc]
________________________________
[物件的型別](https://hackmd.io/FUdLu18IQMmuktnvi3TW9g)</br>
[Method,Block](https://hackmd.io/frjHmYnAQJu7eVt6_RkGNw)</br>
[Class,Model](https://hackmd.io/olFTx4vXSuePrUEswC4ROw)</br>

# 變數種類：
| 類型 | 範例 | 預設值 | 說明 |
| --- | --- | --- | --- |
| 區域變數 （local variable） | name | undefined | 非大寫字母開頭的名字 |
| 全域變數 （global variable） | $name | nil | 前面加了 $ 符號的變數 |
| 實體變數 （instance variable） | @name | nil | 前面加了 @ 符號的變數 |
| 類別變數 （class variable） | @@name | undefined | 前面加了 2 個 @ 符號的變數 |
| 常數 | Name | undefined | 開頭為大寫 |

常數或變數，本身不具型態，只是個描述

# Nil

```ruby
#nil = NilClass
puts nil.class # => NilClass

#false = FalseClass
puts false.class # => FlaseClass
```

# If

```ruby
if age >= 18
	puts "你是大人了"
end

#等同於

puts "你是大人了" if age >= 18
```

# Unless( if not )

```ruby
if not is_adult?(20) #is_adult為自訂義寫法
  puts "你是大人了"
end

#等同於

unless is_adult?(20)
  puts "你是大人了"
end
```

# 三元運算子

```ruby
gender = 1

if gender == 1
  title = "先生"
else
  title = "小姐"
end

puts title   # => 先生

#使用三元運算子

gender = 1
title = (gender == 1) ? "先生" : "小姐"
puts title   # => 先生
```

# Case( 像switch )

```ruby
weather = "下雨"

case weather
when "下雨"
  puts "待在家!"
when "出太陽"
  puts "出去玩!"
else
  puts "在家睡覺!"
end
```

# Range

```ruby
age = 10

if age >= 0 && age <= 3
  puts "Baby"
elsif age >= 4 && age <= 10
  puts "Kids"
elsif age >= 11 && age <= 17
  puts "Teenager"
else
  puts "Adult"
end

#使用range改寫
age = 10

case age
when 0..3
  puts "Baby"
when 4..10
  puts "Kids"
when 11..17
  puts "Teenager"
else
  puts "Adult"
end
```

# 迴圈及迭代（Loop and Iteration）

### while

```ruby

counter = 0

while counter < 5
  puts "hi, #{counter}"
  counter += 1
end

# 執行後得到結果：
# hi, 0
# hi, 1
# hi, 2
# hi, 3
# hi, 4
```

### until( while not )

```ruby
counter = 0

until counter >= 5
  puts "hi, #{counter}"
  counter += 1
end
```

### for...in

```ruby
friends = ["eddie", "joanne", "john", "mark"]

for friend in friends
  puts friend
end

# 執行後得到結果：
# eddie
# joanne
# john
# mark

for i in 1..5 do
  puts i
end

# 執行結果
# 1
# 2
# 3
# 4
# 5
```

### ****times, upto, downto****

```ruby
5.times do
  puts "hello, ruby"
end

# 執行後得到結果：
# hello, ruby
# hello, ruby
# hello, ruby
# hello, ruby
# hello, ruby

#正向從 1 印到 5

1.upto(5) do |i|
  puts "hi, ruby #{i}"
end

# 執行後得到結果：
# hi, ruby 1
# hi, ruby 2
# hi, ruby 3
# hi, ruby 4
# hi, ruby 5

#反向從 5 印到 1

5.downto(1) do |i|
  puts "hi, ruby #{i}"
end

# 執行後得到結果：
# hi, ruby 5
# hi, ruby 4
# hi, ruby 3
# hi, ruby 2
# hi, ruby 1
```

### ****迭代(iteration)****

```ruby
friends = ["eddie", "joanne", "john", "mark"]

friends.each do |friend|
  puts friend
end
```

