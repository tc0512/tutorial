# Python字典入门
## 1 基本语法
```python
<dictionary name> = {<key1>: <val1>, <key2>: <val2>,...}
```

## 2 字典的创建
```python
usr = {"name": "example", "password": 123456, "token": "MWcrO34CWOJAkchc3uIQbg"}

# 访问
print(usr["name"]) #example
print(usr.get("token", None)) #键不存在就返回后面的值
```

## 3 增删改查
```python
usr = {"name": "example", "password": 123456, "token": "MWcrO34CWOJAkchc3uIQbg"}
usr["2fa_secretkey"] = "CT5Y2B7O357VAHB" #键不存在: 增
usr["password"] = 987654 #键存在: 改
del usr["token"] #删并更改原字典
2fa_disabled = usr.pop("2fa_secretkey") #删并返回值
#查就是访问, 前面刚讲过
```

## 4 遍历
```python
usr = {"name": "example", "password": 123456, "token": "MWcrO34CWOJAkchc3uIQbg"}

for i in usr: #遍历键
    print(i)
#name
#password
#token

for j, k in usr.items(): #键和值
    print(j, k)
#name example
#password 123456
#token MWcrO34CWOJAkchc3uIQbg
```

## 5 常用操作
```python
usr = {"name": "example", "password": 123456, "token": "MWcrO34CWOJAkchc3uIQbg"}
print(len(usr)) #键和值的对数
print(usr.keys()) #所有键
print(usr.values()) #所有值
print(usr.items()) #键和值组成的对 (键值对) 
```
