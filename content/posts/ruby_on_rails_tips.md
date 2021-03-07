---
title: "Ruby_on_rails_tips"
date: 2021-03-07T21:22:31+09:00
draft: false
toc: false
images:
tags:
  - ruby
---
### 在class中访问元素
使用`attr_accessor :foo` 定义的变量`foo`可以不需要`self.`直接访问。
如
```
class Person
  attr_accessor :name
  
  def greeting
    name
  end
end

>> person = Person.new
>> person.name = "maiomi"
>> person.greeting
=> "miaomi"
```

教程中解释上述现象的语句为：
`is created automatically by Active Record based on the name of the corresponding database column`

可以不带`self.`取得但是如果更新不带`self.`的话就会建立一个函数的本地变量，不会对类的属性产生影响。
如
```
class Person
  attr_accessor :name

  def greeting(string)
    name = string
  end
end

>> person = Person.new
>> person.greeting("miaomi")
=> "miaomi"
>> person.name
=> nil
```

使用`self.`之后
```
class Person
  attr_accessor :name

  def greeting(string)
    self.name = string
  end
end

>> person = Person.new
>> person.greeting("miaomi")
=> "miaomi"
>> person.name
=> "miaomi"
```

### BCrypt::Password的用法

判断数据库中的加密过的密码是否与提交的密码匹配，使用处理如下：
```
BCrypt::Password.new(remember_digest).is_password?(remember_token)

or 

BCrypt::Password.new(remember_digest) == (remember_token)
```

这里的`==`被重写过，将提交的password加密后与数据库中的被加密过的密码比较
参考：
https://github.com/codahale/bcrypt-ruby/blob/9f3fb05ba7b3732a87e0305ec635d7773bf1fafb/lib/bcrypt/password.rb#L66
使用例：
https://www.rubydoc.info/github/codahale/bcrypt-ruby/BCrypt/Password

### 在不同网页标签中登录同一用户，其中一个登出后另一个再登出出现`ActionController::InvalidAuthenticityToken`错误

修复方法参考
https://qiita.com/_ayk_study/items/88269643c675fd4ca975



### 其他零碎的知识

1.
```
User.find_by(id: cookies.encrypted[:user_id])
```
where cookies.encrypted[:user_id] automatically decrypts the user id cookie.

2.
不带`@`的变量是普通（非实例）变量 -> `normal (non-instance) variable`
在测试中不能用`assigns(:foo)`来获取

3.
`session` method are automatically encrypted
参考
https://stackoverflow.com/questions/39601527/how-session-works-in-rails

4.
so how does Rails know to use a POST request for new users and a PATCH for editing users? The answer is that it is possible to tell whether a user is new or already exists in the database via Active Record’s new_record? boolean method:
```
$ rails console
>> User.new.new_record?
=> true
>> User.first.new_record?
=> false
```
When constructing a form using form_with(@user), Rails uses POST if @user.new_record? is true and PATCH if it is false.

5.
使用`target="_blank"`进行跳转时的安全问题。
https://imququ.com/post/the-security-of-window-opener-location.html

6.
fixture中的用户保存时不会被model中的`before_save`限制，就导致如果大小写不注意的话测试中的登陆会失败。

7.
`belongs_to`的`foreign_key:`和`primary_key:`选项
`foreign_key:`指定自己存储外键的行（column）的名称
`primary_key:`指定自己存储外键的行中的值来自从属model的哪一行（column）

`has_many`的`foreign_key:`和`primary_key:`选项
`foreign_key:`指定子类存储外键的行（column）的名称
`primary_key:`指定子类存储外键的行中的值来自自己的哪一行（column）

问题：
polymorphic_path 生成的路径会有单数的时候
如/article_latest/1
答：参考下面（用单数重写的话就会保持单数）
https://stackoverflow.com/questions/23931120/how-does-form-for-generate-paths-in-rails

