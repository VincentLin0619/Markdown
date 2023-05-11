# Class,Model
[←回上頁](../Ruby-base.md)

----------------------------------------------------------------
[toc]
----------------------------------------------------------------

# Class

- ****什麼是物件？****

    > 物件（Object）= 狀態（State）+ 行為（Behavior）
    >
- ****什麼是類別？****

    一個描述物件內容的概念

    類別命名需為常數（開頭為大寫）

- ****定義類別****

    ```ruby
    class Cat
    	#...要做的事情
    end

    #實例化
    kitty = Cat.new
    ```

- ****初始化****

    ```ruby
    class Cat
    	def initialize(name, gender) #建構參數
    		@name = name
    		@gender = gender
    	end

    	def say_name
    		puts " My name is #{@name} "
    	end
    end

    kitty = Cat.new("kitty", "female")
    kitty.say_name #=> My name is kitty
    ```

- ****實體變數（instance variable）****

    ```ruby
    class PostsController < ApplicationController
      def index
        @posts = Post.all    # 取得所有的 Post 資料
      end
    end
    ```

- ****取用實體變數****

    實體變數無法從外部取用，因Ruby沒有屬性（property/attribute）這樣的設計，要取用實體變數，需要另外定義的方法

    ```ruby
    class Cat
      def initialize(name, gender)
        @name = name
        @gender = gender
      end

      def say_hello
        puts "hello, my name is #{@name}"
      end

      def name
        @name
      end

      def name=(new_name)
        @name = new_name
      end
    end

    kitty = Cat.new("kitty", "female")
    kitty.name = "nancy"  ##此處調用的方法是 name= 並非 name
    puts kitty.name        # => nancy
    ```

- Ruby預設的**attr方法reader, writer, accessor**

    attr_reader ⇒ 讀取

    attr_writer ⇒ 設定

    attr_accessor ⇒ 讀取、設定

    ```ruby
    class Cat
      attr_accessor :name

      def initialize(name, gender)
        @name = name
        @gender = gender
      end

      def say_hello
        puts "hello, my name is #{@name}"
      end
    end
    ```

- ****實體方法與類別方法****

    依據作用的對象不同，有分實體方法（instance method）及類別方法（class method）

    ```ruby
    kitty = Cat.new("kitty", "female")
    kitty.say_hello ###<= say_hello就是作用在kitty的實體

    class PostsController < ApplicationController
      def index
        @posts = Post.all    # 取得所有的 Post 資料
                             # all作用在Post這個類別上，所以為類別方法
      end
    end
    ```

    Ruby要定義類別有幾種寫法

    常見的為使用self：

    ```ruby
    class Cat
      def self.all
        # ...
      end
    end

    #要調用時使用call呼叫 => Cat.call

    #=============另外一種寫法

    class Cat
    	class << self
    		def all
    			#...
    		end
    	end
    end
    ```


## 方法的存取控制

public - 所有人都能直接存取

private - 只有內部類別才能存取

protected - 兩者之間，比 private 寬鬆一些，但又沒有 public 那麼自在，protected 在同一個類別內或是同一個 package，或是繼承它的子類別可以自由取用，但如果不是的話則不可存取。

Ruby設定的方式：

```ruby
#寫在定義之前
class Cat
  def eat
    puts "好吃!"
  end

  protected
  def sleeping
    puts "zzzzzzzzz..."
  end

  private
  def gossip
    puts "我跟你說，你不要跟別人說喔!"
  end
end

#寫在定義之後
class Cat
  def eat
    puts "好吃!"
  end

  def sleeping
    puts "zzzzzzzzz..."
  end

  def gossip
    puts "我跟你說，你不要跟別人說喔!"
  end

  protected :sleeping
  private :gossip
end
```

Ruby 冷知識：

> 沒有特別限制的方法預設都是 public，但除了一個例外，就是負責初始化的initialize方法，它永遠是 private 的，只會被new方法呼叫。而這三者都不是關鍵字，只是一般方法。
>

與其他語言不同的特性：

```ruby
kitty = Cat.new
kitty.eat           # => "好吃!"
kitty.sleeping      # => NoMethodError
kitty.gossip        # => NoMethodError

##這裡的 kitty.eat
##不是單純「kitty物件調用了eat方法」而已
##Ruby解讀為「有一個kitty物件，發送了eat這項訊息（message)，而kitty就是這個訊息的receiver」
```

### Private?

```ruby
class Cat
  def say_hello
    self.gossip
  end

  private
  def gossip
    puts "我跟你說，你不要跟別人說喔!"
  end
end

kitty = Cat.new
kitty.say_hello        # NoMethodError
kitty.gossip          # => NoMethodError

#在類別裡沒有明確的接收者，所以上述都會出現錯誤，但在下例程式碼，使用send把gossip當成參數傳遞給kitty就沒有違反規定

kitty.send(:gossip)   # => 我跟你說，你不要跟別人說喔!

```

- ****繼承（Inheritance）****

    ```ruby
    class Cat
      def eat(food)
        puts "#{food} 好好吃!!"
      end
    end

    class Dog
      def eat(food)
        puts "#{food} 好好吃!!"
      end
    end

    #以上Cat,Dog類別裡都有定義eat的方法
    ```

    在OOP概念裡，重複的方法應該獨立出來定義，再去繼承他

    ```ruby
    class Animal
    	def eat(food)
    		puts "#{food} 好好吃！！"
    	end
    end

    class Cat < Animal
    end

    class Dog < Animal
    end
    ```

    有重複名稱的類別，將會融合

    ```ruby
    class Cat
      def abc
        # ...
      end
    end

    class Cat
      def xyz
        # ...
      end
    end

    #會變成
    class Cat
      def abc
        # ...
      end

      def xyz
        # ...
      end
    end
    ```

    Open Class（開放類別）

    ```ruby
    class String
    	def say_hello
    		"hi, I am #{self}"
    	end
    end

    puts "eddie".say_hello  # => hi, I am eddie
    puts "kitty".say_hello  # => hi, I am kitty
    ```


# 模組（Module）

- ****類別跟模組寫起來好像？****

    ```ruby
    #宣告模組
    module Flyable
    	def fly(name)
    		puts "#{name} can fly!!"
    	end
    end

    class Cat
    	include Flyable
    end

    kitty = Cat.new
    kitty.fly('Kitty')  #=> " Kitty can Fly "
    ```

    模組為類別的上層，與類別不同的地方為

    1. 模組無法實體化
    2. 模組無法繼承模組

    > 如果你發現你要做的這個功能，它可能在很多不同體系的類別裡都會用得到，那你可以考慮把功能寫在模組裡，需要的時候再 include 進來即可。但如果你還是不知道到底類別跟模組有什麼差別，我再舉二個例子。不知道大家有沒看過[火影忍者](https://goo.gl/4zDQga)這部漫畫，漫畫裡的主人公之一，宇智波佐助，因為他們家族血統的關係，他寫輪眼這個功能是天生就有的，你可以想像他這個功能算是從他的家族「繼承」來的。而佐助的老師，旗木卡卡西，他雖然也有寫輪眼功能，但他的寫輪眼並非繼承來的，事實上是他在年輕時候include 了某位朋友的寫輪眼模組，所以才有這個效果。另一個例子，[海賊王](https://zh.wikipedia.org/wiki/ONE_PIECE)漫畫裡，主角魯夫本來是普通人，但在偶然的機會下，他 include 了橡膠果實之後，他就有了橡膠人的能力了，並不是因為他老爸是橡膠人所以他才是橡膠人。
    >
- ****在 Rails 專案中，模組用在哪些地方？****

**到家**
