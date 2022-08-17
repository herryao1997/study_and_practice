# 题目1 （类的定义与调用）：  
## 1. 构造一个银联信用卡的类：
- 包含如下属性：  
顾客姓名  
信用卡授信额度  
当前额度  
单词刷卡金额上限
- 包含如下方法：  
分别获取上述属性的方法  
对授信额度进行修改的方法  
对单词刷卡金额上限进行修改的方法  
实现刷卡方法，传入一个刷卡金额判断是否超过单次刷卡金额上限以及当前额度是否够用，若合理执行刷卡，将当前额度减去刷卡金额
- 创建信用卡实例
执行调用相应的属性和方法


```python
class CreditCard():
    """模拟信用卡"""
    
    def __init__(self, name, total_limit, current_limit, limit_for_payment):
        """定义信用卡的属性"""
        self.name = name                               #用户姓名
        self.total_limit = total_limit        #授信额度
        self.current_limit = current_limit             #当前额度
        self.limit_for_payment = limit_for_payment     #单词刷卡上限
        
    def get_information(self):
        """获取所有属性的方法"""
        print("用户姓名为：{}，授信额度为：{}，当前额度为：{}，单次刷卡上限为：{}"
              .format(self.name, self.total_limit, self.current_limit, self.limit_for_payment))
        
    def set_total_limit(self, limit):
        """修改授信额度"""
        if limit > 0:
            temp = self.total_limit
            self.total_limit = limit
            return temp
        else:
            print("错误输入！")
    
    def set_limit_for_payment(self, limit):
        """修改单次刷卡上限"""
        if limit > 0 and limit < self.total_limit:
            temp = self.total_limit
            self.limit_for_payment = limit
            return temp
        else:
            print("输入金额无效或超出授信额度")
    
    def payment(self, payment):
        """实现刷卡"""
        if payment <= self.limit_for_payment:
            if payment <= self.current_limit:
                temp = self.current_limit
                self.current_limit -= payment
            else:
                print("超剩余额度")
        else:
            print("超出单日额度")        
```


```python
def start():
    user = CreditCard("herr", 100, 100, 5)
    user.get_information()
    user.set_limit_for_payment(300)
    user.set_total_limit(-50000)
    user.set_total_limit(50000)
    user.set_limit_for_payment(500)
    user.get_information()
    user.payment(10)
    user.get_information()
    user.payment(1000)
    user.payment(100)
    
if __name__ == "__main__":
    start()
```

    用户姓名为：herr，授信额度为：100，当前额度为：100，单次刷卡上限为：5
    输入金额无效或超出授信额度
    错误输入！
    用户姓名为：herr，授信额度为：50000，当前额度为：100，单次刷卡上限为：500
    用户姓名为：herr，授信额度为：50000，当前额度为：90，单次刷卡上限为：500
    超出单日额度
    超剩余额度


# 题目二（类的继承）
## 2. 通过继承银联信用卡的类，构造中国银行信用卡的类：
- 实现对银联信用卡类的继承
- 新增加属性：中国银行卡信用卡积分、优惠店铺列表
- 重写刷卡方法：
传入消费店铺和消费金额，如果店铺名称在优惠店铺列表中，则刷卡金额打95折  
每消费10元，信用卡积分增加1分  
保留父类刷卡方法及其他功能  
- 新增如下方法：  
获得用户积分的方法  
设置优惠店铺列表的方法


```python
class CreditCard():
    """模拟信用卡"""
    
    def __init__(self, name, total_limit, current_limit, limit_for_payment):
        """定义信用卡的属性"""
        self.name = name                               #用户姓名
        self.total_limit = total_limit        #授信额度
        self.current_limit = current_limit             #当前额度
        self.limit_for_payment = limit_for_payment     #单词刷卡上限
        
    def get_information(self):
        """获取所有属性的方法"""
        print("用户姓名为：{}，授信额度为：{}，当前额度为：{}，单次刷卡上限为：{}"
              .format(self.name, self.total_limit, self.current_limit, self.limit_for_payment))
        
    def set_total_limit(self, limit):
        """修改授信额度"""
        if limit > 0:
            temp = self.total_limit
            self.total_limit = limit
            return temp
        else:
            print("错误输入！")
    
    def set_limit_for_payment(self, limit):
        """修改单次刷卡上限"""
        if limit > 0 and limit < self.total_limit:
            temp = self.total_limit
            self.limit_for_payment = limit
            return temp
        else:
            print("输入金额无效或超出授信额度")
    
    def payment(self, payment):
        """实现刷卡"""
        if payment <= self.limit_for_payment:
            if payment <= self.current_limit:
                temp = self.current_limit
                self.current_limit -= payment
            else:
                print("超剩余额度")
        else:
            print("超出单日额度")
```


```python
class ChinaCredit(CreditCard):
    """构建中国银行信用卡"""
    
    def __init__(self, name, total_limit, current_limit, limit_for_payment):
        super().__init__(name, total_limit, current_limit, limit_for_payment)
        self.points = 0
        self.discount_list = ["华为","小米","联想"]
        
    def payment(self, payment, store):
        """重写刷卡"""
        if store in self.discount_list:
            discount = 0.95
            if payment <= self.limit_for_payment:
                if payment <= self.current_limit:
                    temp = self.current_limit
                    payment *= discount
                    self.current_limit -= payment
                    self.points += payment // 10
                else:
                    print("超剩余额度")
            else:
                print("超出单日额度")        
        
    def set_store(self, new_store):
        """添加店铺"""
        self.discount_list.append(new_store)
        return self.discount_list
    
    def get_point(self):
        """获取积分信息"""
        print("剩余积分为：{}".format(self.points))
```


```python
def start():
    user = ChinaCredit("herr", 10000, 5000, 500)
    user.get_information()
    user.get_point()
    user.payment(1000, "华为")
    user.set_limit_for_payment(2000)
    user.payment(1000, "华为")
    user.get_information()
    user.get_point()
    
if __name__ == "__main__":
    start()
```

    用户姓名为：herr，授信额度为：10000，当前额度为：5000，单次刷卡上限为：500
    剩余积分为：0
    超出单日额度
    用户姓名为：herr，授信额度为：10000，当前额度为：4050.0，单次刷卡上限为：2000
    剩余积分为：95.0

