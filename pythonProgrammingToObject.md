# 引子：  
为什么要引进面向对象的变成： 面向对象更加符合人类对于客观世界的抽象和理解
- 一切皆对象
- 一切的对象都存在自身的内在属性
- 一切行为都是来自与对象的行为
因此可见面向过程的编程很难实现以上的需求，因次引入了面向对象的编程  
- 如何实现面向对象的编程呢
在计算机语言中，类是对象的载体  
把一类对象的公共特征抽象出来，创建通用的类  

创建类的简单的例子


```python
class Cat():
    """模拟猫"""
    
    def __init__(self, name):
        """初始化属性"""
        self.name = name
    def jump(self):
        """模拟动作"""
        print(self.name + " is jumpping")
```


```python
#用类来创建的实例
my_cat = Cat("tom")
your_cat = Cat("jerry")
```


```python
#调用属性
print(my_cat.name)
print(your_cat.name)
```

    tom
    jerry



```python
#调用方法
my_cat.jump()
your_cat.jump()
```

    tom is jumpping
    jerry is jumpping


## 类的定义的三要素
1. 类的命名
2. 类的属性
3. 类的方法

### 类的命名  
- 要有实际的意义
- 驼峰命名法：组成的单词大写首字母
eg：Dog、BadBoy


```python
#类名定义习惯  
"""定义类之前一般空两行"""


class Car():
    """对类进行简单的介绍"""
    pass

#定义类以后也要空两行
```

### 类的属性  
- 通过传入的参数进行类属性的初始化
- 在属性内部自定义初始化，此时可以不用定义参数传递，需要修改的时候可以通过类属性赋值对其进行后期修改


```python
#def __init__(self, 参数): 初始化类的属性
```


```python
class Car():
    """模拟汽车"""
    def __init__(self, brand, model, year):
        """初始化汽车类的属性"""
        self.brand = brand    #汽车的品牌属性
        self.model = model    #汽车的型号属性
        self.year = year      #汽车的出场年份属性
        self.mileage = 0      #汽车的里程数
```

### 类的方法
- 下面例子定义了一个类，并赋予了其四个类的属性和两个类的方法


```python
#def 方法名称(self):
```


```python
class Car():
    '''模拟汽车'''
    def __init__(self, brand, model, year):
        """初始化汽车的属性"""
        self.brand = brand    #汽车的品牌属性
        self.model = model    #汽车的型号属性
        self.year = year      #汽车的出场年份属性
        self.mileage = 0      #汽车的里程数
    def get_main_information(self):
        """获取汽车的主要信息"""
        print("品牌：{}\n型号：{}\n出场年份{}\n".format(self.brand,self.model,self.year))
    def get_mileage(self):
        """获取总里程数"""
        return "里程数：{}".format(self.mileage)
```

## 创建实例  
- 创建实例要把类的属性全部进行初始化
- 初始化的不足和过量均会导致语法错误

### 实例的创建  
将实例赋值给对象，再次过程冲传入相应的参数  
实例名称 = 类名称(必要的初始化参数)


```python
my_new_car = Car("BMW", 530, 2021)
```

### 属性的访问  
类名.属性名


```python
print(my_new_car.brand)
print(my_new_car.model)
print(my_new_car.year)
```

    BMW
    530
    2021


### 调用的方法  
实例名.方法名(必要的参数)


```python
# my_new_car = Car("BMW", 530, 2021)
my_new_car.get_main_information()
s = my_new_car.get_mileage()
print(s)
```

    品牌：BMW
    型号：530
    出场年份2021
    
    里程数：0


### 修改属性
- 直接修改
- 通过方法修改

### 直接修改法


```python
#直接修改
my_old_car = Car("BYD", "F0", 2001)
```


```python
#先访问后修改
print(my_old_car.mileage)
my_old_car.mileage = 100000
print(my_old_car.mileage)
```

    0
    100000



```python
print(my_old_car.get_mileage())
```

    里程数：100000


### 通过方法修改


```python
class Car():
    def __init__(self, brand, model, year):
        """初始化汽车属性"""
        self.brand = brand    #汽车的品牌属性
        self.model = model    #汽车的型号属性
        self.year = year      #汽车的出场年份属性
        self.mileage = 0      #汽车的里程数
        
    def get_main_information(self):
        """获取汽车的主要功能"""
        print("品牌：{}\n型号：{}\n出场日期：{}\n".format(self.brand, self.model, self.year))
        
    def get_mileage(self):
        """获取汽车的里程数"""
        return("汽车行驶里程数：为{}公里\n".format(self.mileage))
    
    def set_mileage(self, distance):         
        """获取汽车的总里程数"""              
        self.mileage = distance            
```


```python
my_old_car = Car("BYD", "F3", 2016)
print(my_old_car.get_mileage())
my_old_car.set_mileage(8000)
print(my_old_car.get_mileage())
```

    汽车行驶里程数：为0公里
    
    汽车行驶里程数：为8000公里
    


### 进一步拓展  
禁止输出负里程数


```python
class Car():
    """模拟汽车"""
    def __init__(self, brand, model, year):
        """初始化类属性"""
        self.brand = brand    #汽车的品牌属性
        self.model = model    #汽车的型号属性
        self.year = year      #汽车的出场年份属性
        self.mileage = 0      #汽车的里程数
        
    def get_main_information(self):
        """获取汽车的主要信息"""
        print("品牌为{}\n型号：{}\n出场日期：{}\n".format(self.brand, self.model, self.year))
    
    def get_mileage(self):
        """获取汽车的里程数"""
        return("汽车行驶里程数：为{}公里\n".format(self.mileage))
        
    def set_mileage(self, distance):
        if distance >= 0:
            self.mileage = distance
        else:
            print("里程数不可为负值")
        
    def increment_mileage(self, distance):
        if distance >= 0:
            self.mileage += distance
        else:
            print("里程增量不可为负值")
```


```python
my_old_car = Car("BYD", "F3", 2016)
print(my_old_car.get_mileage())
my_old_car.set_mileage(-8000)
print(my_old_car.get_mileage())
```

    汽车行驶里程数：为0公里
    
    里程数不可为负值
    汽车行驶里程数：为0公里
    



```python
my_old_car = Car("BYD", "F3", 2016)
print(my_old_car.get_mileage())
my_old_car.set_mileage(8000)
print(my_old_car.get_mileage())
my_old_car.increment_mileage(500)
print(my_old_car.get_mileage())
```

    汽车行驶里程数：为0公里
    
    汽车行驶里程数：为8000公里
    
    汽车行驶里程数：为8500公里
    


## 小结


```python
my_new_car = Car("BMW", 530, 2021)
my_old_car = Car("BYD", "F3", 2016)
my_cars = [my_new_car, my_old_car]
```

- 由此可以看出结合类，信息的包含量是巨大的，几乎可以创建无穷多的实例
- 以人的视角观看事物的规律和变化，更容易理解
