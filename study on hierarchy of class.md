# 类的继承问题

## 原因  
- 多个相似的类别分别构建会大大降低效率，形成冗余的代码，也会降低可读性  
- 将各种类别的**公共特征**提取出来形成一个公共大类  
- 给中子类继承父类的公共特征，并添加自身的特殊特征，从而形成一个子类  
- **所谓继承，即底层抽象继承高层抽象的过程**


## 简单的继承问题

### 父类


```python
class Car():
    """模拟汽车"""
    def __init__(self, brand, model, year):
        """初始化汽车属性"""
        self.brand = brand    #相当于类内部的变量
        self.model = model    #汽车的型号属性
        self.year = year      #汽车的出场年份属性
        self.mileage = 0      #汽车的里程数
    
    def get_main_information(self):
        """获取汽车的主要信息"""
        print("品牌：{}\n型号：{}\n出厂年份{}\n".format(self.brand, self.model, self.year))
        
    def get_mileage(self):
        """获取总里程数"""
        return("里程数：{}".format(self.mileage))
    
    def set_mileage(self, distance):
        if distance >= 0:
            self_mileage = distance
        else:
            print("里程数不可为负值")
            
    def increment_mileage(self, distance):
        if distance >= 0:
            self_mileage = distance
        else:
            print("里程增加数不可为负值")
```

### 子类

**class 子类(父类):**  
- 新建一个电动车类


```python
class ElectricCar(Car):
    """模拟电动车"""
    def __init__(self, brand, model, year):
        """初始化电动车属性"""
        super().__init__(brand, model, year) #声明父类的属性，注意此处没有声明self参数，所有父类的属性和方法自动继承
```

- 以上子类定义其实为父类的完全复制，并没有更新子类特有的属性和方法  
- 声明父类以后，不仅仅继承了父类的属性，方法也全部继承过来  
- 如果子类中定义属性或方法与父类产生冲突优先服从子类设定  
- 计算机收到代码的指令调用类的属性或类的方法优先从子类检索，子类没有再去父类检索

### 给子类添加方法：


```python
class ElectricCar(Car):
    def __init__(self, brand, model, year, battery_size):
        """模拟电动车"""
        super().__init__(brand, model, year)  #声明父类的属性，注意此处没有声明self参数，所有父类的属性和方法自动继承
        self.battery_size = battery_size      #电池容量
        self.electric_quantity = battery_size #初始化电池剩余容量为电池容量
        self.electric2distance_ratio = 5      #电量距离转换比
        self.remain_distance = self.electric_quantity * self.electric2distance_ratio #剩余距离
        
    def get_electric_quantity(self):
        """查看当前剩余电量"""
        print("当前剩余电量为{}".format(self.electric_quantity))
        
    def set_electric_quantity(self, electric_quantity):
        """输入剩余电量，重新计算剩余里程数"""
        if electric_quantity >= 0:
            self.electric_quantity = electric_quantity
            self.remain_distance = self.electric_quantity * self.electric2distance_ratio     #更新电量和里程数
        else:
            print("电量不能为负数")
            
    def get_remain_distance(self):
        """计算剩余可行驶里程数"""
        print("剩余可行驶公里数为：{}".format(self.remain_distance))
        
```


```python
my_electric_car = ElectricCar("xiao", "swd", 2045, 70)
my_electric_car.get_main_information()      #获取信息（父类）
my_electric_car.get_electric_quantity()     #获取当前电池电量 
my_electric_car.get_remain_distance()       #获取剩余里程数
```

    品牌：xiao
    型号：swd
    出厂年份2045
    
    当前剩余电量为70
    剩余可行驶公里数为：350



```python
my_electric_car.set_electric_quantity(60)    #修改电池剩余电量
my_electric_car.get_electric_quantity()
my_electric_car.get_remain_distance()        #显示修改后的信息
```

    当前剩余电量为60
    剩余可行驶公里数为：300


### 重写父类的方法——多态法


```python
class ElectricCar(Car):
    def __init__(self, brand, model, year, battery_size):
        """模拟电动车"""
        super().__init__(brand, model, year)  #声明父类的属性，注意此处没有声明self参数，所有父类的属性和方法自动继承
        self.battery_size = battery_size      #电池容量
        self.electric_quantity = battery_size #初始化电池剩余容量为电池容量
        self.electric2distance_ratio = 5      #电量距离转换比
        self.remain_distance = self.electric_quantity * self.electric2distance_ratio #剩余距离
        
    def get_main_information(self):           #重写父类方法
        """获取汽车主要信息"""
        print("品牌：{}\n型号：{}\n出厂年份：{}\n续航里程：{}\n"
              .format(self.brand, self.model, self.year, self.remain_distance))
    
    
    def get_electric_quantity(self):
        """查看当前剩余电量"""
        print("当前剩余电量为{}".format(self.electric_quantity))
        
    def set_electric_quantity(self, electric_quantity):
        """输入剩余电量，重新计算剩余里程数"""
        if electric_quantity >= 0:
            self.electric_quantity = electric_quantity
            self.remain_distance = self.electric_quantity * self.electric2distance_ratio     #更新电量和里程数
        else:
            print("电量不能为负数")
            
    def get_remain_distance(self):
        """计算剩余可行驶里程数"""
        print("剩余可行驶公里数为：{}".format(self.remain_distance))
```


```python
my_electric_car = ElectricCar("xiao", "swd", 2045, 70)
my_electric_car.get_main_information()      #获取信息（经过子类修改）
my_electric_car.set_electric_quantity(60)   #修改电池剩余电量
my_electric_car.get_main_information()      #获取信息（电量修改之后）（经过子类修改父类）
```

    品牌：xiao
    型号：swd
    出厂年份：2045
    续航里程：350
    
    品牌：xiao
    型号：swd
    出厂年份：2045
    续航里程：300
    


### 用在类中的一个实例
将电池抽象为一个对象  
逻辑更加清晰


```python
class Battery():
    """模拟电池"""
    
    def __init__(self, battery_size = 70):
        self.battery_size = battery_size      #电池容量
        self.electric_quantity = battery_size #初始化电池剩余容量为电池容量
        self.electric2distance_ratio = 5      #电量距离转换比
        self.remain_distance = self.electric_quantity * self.electric2distance_ratio #剩余距离
        
    def get_electric_quantity(self):
        """查看当前剩余电量"""
        print("当前剩余电量为{}".format(self.electric_quantity))
        
    def set_electric_quantity(self, electric_quantity):
        """输入剩余电量，重新计算剩余里程数"""
        if electric_quantity >= 0:
            self.electric_quantity = electric_quantity
            self.remain_distance = self.electric_quantity * self.electric2distance_ratio     #更新电量和里程数
        else:
            print("电量不能为负数")
            
    def get_remain_distance(self):
        """计算剩余可行驶里程数"""
        print("剩余可行驶公里数为：{}".format(self.remain_distance))
```


```python
class ElectricCar(Car):
    """模拟电动汽车"""
    
    def __init__(self, brand, model, year, electric_quantity):
        """初始化电动汽车属性"""
        super().__init__(brand, model, year)       #声明父类的属性，注意此处没有声明self参数，所有父类的属性和方法自动继承
        self.battery = Battery(electric_quantity)  #电池
    
    def get_main_information(self):           #重写父类方法
        """获取汽车主要信息"""
        print("品牌：{}\n型号：{}\n出厂年份：{}\n续航里程：{}\n"
              .format(self.brand, self.model, self.year, self.battery.remain_distance))     
    
        
```


```python
my_electric_car = ElectricCar("xiao", "swd", 2045, 70)
my_electric_car.get_main_information()      #获取信息
```

    品牌：xiao
    型号：swd
    出厂年份：2045
    续航里程：350
    



```python
my_electric_car.battery.set_electric_quantity(30)  #利用类访问
my_electric_car.get_main_information()             #获取信息
```

    品牌：xiao
    型号：swd
    出厂年份：2045
    续航里程：150
    

