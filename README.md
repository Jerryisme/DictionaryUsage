# DictionaryUsage
字典的使用

![](https://upload-images.jianshu.io/upload_images/2198604-f8238e3b88d0d681.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### [NSDictionary官方API文档](https://developer.apple.com/documentation/foundation/nsdictionary?language=objc)

### [NSMutableDictionary官方API文档](https://developer.apple.com/documentation/foundation/nsmutabledictionary?language=objc)

## 常用方法

### 不可变字典

``` 
#pragma mark - 不可变字典
- (void)setUpDictionary {
    // 字典  所以 ---> 文字内容（key ---> value）  里面存储的都是键值对
    NSDictionary *dic0 = [NSDictionary dictionary];
    NSLog(@"创建空字典---    %@\n", dic0);
    
    NSDictionary *dic1 = [NSDictionary dictionaryWithObject:@"jerry" forKey:@"name"];
    NSLog(@"创建字典dic1---    %@\n", dic1);
    
    // 查找key对应的value
    id obj1 = [dic1 objectForKey:@"name"];
    NSLog(@"查找key对应的value obj1---    %@\n", obj1);
    
    NSDictionary *dic2 = [NSDictionary dictionaryWithObjects:@[@"jerry", @"3"] forKeys:@[@"name", @"old"]];
    NSLog(@"创建字典dic2---    %@\n", dic2);
    
    id obj2 = [dic2 objectForKey:@"old"];
    NSLog(@"查找key对应的value obj2---    %@\n", obj2);
    
    // 很少用
    NSDictionary *dic3 = [NSDictionary dictionaryWithObjectsAndKeys:@"jerry", @"name", @"3", @"old", @"man", @"sex", nil];
    NSLog(@"创建字典dic3---    %@\n", dic3);
    
    id obj3 = [dic3 objectForKey:@"sex"];
    NSLog(@"查找key对应的value obj3---    %@\n", obj3);
    
    NSDictionary *dic4 = @{@"address":@"shanghai", @"birthday":@"2000-01-01"};
    NSLog(@"创建字典dic4---    %@\n", dic4);
    
    id obj4 = dic4[@"address"];
    NSLog(@"查找key对应的value obj4---    %@\n", obj4);
    
    // 获取字典键值对个数
    NSLog(@"获取字典键值对个数---    %ld\n", dic4.count);
    
    // 获取字典中所有key
     NSLog(@"获取字典中所有key---    %@\n", [dic4 allKeys]);
    NSEnumerator *enumKeys = [dic4 keyEnumerator];
    for (NSObject *obj in enumKeys){
        NSLog(@"enumKey: %@", obj);
    }
    for (NSObject *obj in dic4){
        NSLog(@"key in dict: %@", obj);
    }
    
    // 获取字典中所有value
     NSLog(@"获取字典中所有value---    %@\n", [dic4 allValues]);
    NSEnumerator *enumValues = [dic4 objectEnumerator];
    for (NSObject *obj in enumValues){
        NSLog(@"value in dict: %@", obj);
    }
    
    // 字典写入沙盒中，沙盒中读取字典
    [self writeAndContents];
}
```

### 可变字典

```
#pragma mark - 可变字典
- (void)setUpNSMutableDictionary {
    NSMutableDictionary *mutableDic = [@{@"address":@"shanghai", @"birthday":@"2000-01-01", @"name":@"haha"} mutableCopy];
    NSLog(@"创建字典mutableDic---    %@\n", mutableDic);
    
    // 添加键值对
    NSMutableDictionary *mutableDic0 = [NSMutableDictionary dictionary];
    [mutableDic0 setObject:@"jerry" forKey:@"name"];
    [mutableDic0 setObject:@"3" forKey:@"old"];
    [mutableDic0 setObject:@"tom" forKey:@"name"];
    
    id obj = mutableDic0[@"name"];
    NSLog(@"查找key对应的value obj---    %@\n", obj);
    
    // 将一个字典键值添加到另外一个字典中，如果B里面key如果A里面存在相同的key的话也会替换value
    [mutableDic addEntriesFromDictionary:mutableDic0];
    NSLog(@"将一个字典键值添加到另外一个字典中，如果B里面key如果A里面存在相同的key的话也会替换valuemutableDic---    %@\n", mutableDic);
    
    [mutableDic setValuesForKeysWithDictionary:@{@"love":@"eat", @"name":@"TT"}];
    NSLog(@"把B里面键值对添加字典A里面---    %@\n", mutableDic);
    
    // 替换整个字典
    [mutableDic setDictionary:mutableDic0];
    NSLog(@"替换整个字典---    %@\n", mutableDic);
    
    // 可以指定“键”的方式删除字典中对应的“键值对”，如果该Key不存在，则不做任何操作
    [mutableDic0 removeObjectForKey:@"old"];
    
    // 清空整个字典的数据
    [mutableDic0 removeAllObjects];
}
```

### 字典写入沙盒中，沙盒中读取字典

```
#pragma mark - 字典写入沙盒中，沙盒中读取字典
- (void)writeAndContents {
    // 获取Documents目录
    NSString *docPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    // 字典写入文件
    // 创建一个存储字典的文件路径
    NSString *fileDicPath = [docPath stringByAppendingPathComponent:@"bug.txt"];
    NSDictionary *dic = @{@"name":@"jerry", @"old":@"3"};
    
    // 字典写入时执行的方法
    [dic writeToFile:fileDicPath atomically:YES];
    NSLog(@"字典写入时执行的方法---    %@\n", fileDicPath);
    
    // 从文件中读取数据字典的方法
    NSDictionary *resultDic = [NSDictionary dictionaryWithContentsOfFile:fileDicPath];
    NSLog(@"从文件中读取数据字典的方法---    %@\n", resultDic);
}
```

### 字典转化字符串

```
#pragma mark - 字典转化字符串
+ (NSString*)dictionaryToJson:(NSDictionary *)dic
{
    NSError *parseError = nil;
    NSData *jsonData = [NSJSONSerialization dataWithJSONObject:dic options:NSJSONWritingPrettyPrinted error:&parseError];
    
    return [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];
}
```

### 字符串转字典

```
#pragma mark - 字符串转字典
+ (NSDictionary *)dictionaryWithJsonString:(NSString *)jsonString
{
    if (jsonString == nil) {
        return nil;
    }
    NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
    NSError *err;
    NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:jsonData options:NSJSONReadingMutableContainers error:&err];
    if(err) {
        NSLog(@"json解析失败：%@",err);
        return nil;
    }
    return dic;
}
```

#### 最后,觉得有用记得给个star✨!非常感谢!

##### 简书地址:https://www.jianshu.com/p/e71c40547b1b

##### 简书个人主页:https://www.jianshu.com/u/281c41cc90bc
