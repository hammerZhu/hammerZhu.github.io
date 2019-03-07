---
layout: post
title: ios基本控件
categories: ios
description: UIView，UILabel，UIButton等
keywords: ios objc
---
本文描述了常用的几种基本控件的设置方法

#控件之间的关系

列出类图


# UIView属性

# UIControl属性
UIControl就是在UIView基础上，增加了事件处理等。

## 事件处理
UIButton* btn
//注册一个点击事件
[btn addTarget:self action:@selector(colorfulOnOff:) forControlEvents:UIControlEventTouchUpInside];
具体事件的种类根据控件类型不同有所区别。

## 添加自定义事件

todo



# UILabel
标签控件

```objc
UILabel *label1=[[UILabel alloc] initWithFrame:CGRectMake(0,10,60,40)]; //创建label 
label1.backgroundColor = [ UIColor grayColor];
label1.text = @"disconnected";//设置标签文本 
label1.font = [ UIFont fontWithName:@"Arial" size:30];//文本字体和文本大小 
label1.textColor = [ UIColor blueColor];  //设置文本颜色
abel1.textAlignment = NSTextAlignmentCenter;    //对齐方式
label1.numberOfLine = 2;         //文本最多行数，为0时没有最大行数限制
label1.minimumFontSize ＝ 10.0；  //最小字体行数为1时有效  默认0.0
label1.highlighted = YES;  //设置文本高亮
label1.enabled ＝ YES；  //文本是否可变  
label1.backgroundColor = [ UIColor clearColor];   //去掉背景色
label1.shadowColor = [ UIColor grayColor];    //文本阴影颜色  
label1.shadowOffset = CGRectMake(1.0,1.0);    //阴影大小
label1.userInteractionEnabled = YES;     //能否与用户交互  
```


# UIButton

## 基本用法

```objc
UIButton *btn = [UIButton buttonWithType:UIButtonTypeSystem];
btn.frame = CGRectMake((wide-240*scale)/2, 130*scale,240*scale, 40*scale);
btn.layer.cornerRadius=5;
btn.backgroundColor = [ UIColor blueColor];//背景色
//标题颜色，字体，内容。
btn.titleLabel.font = [UIFont systemFontOfSize: 24.0];
[btn setTitle:@"send password to mailbox" forState:UIControlStateNormal];
[btn setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
[btn addTarget:self action:@selector(colorfulOnOff:) forControlEvents:UIControlEventTouchUpInside];
//事件 
-(void)colorfulOnOff:(id)sender
{
}//@selector可以理解为"选择子"，selector是一个指针变量，类似于sender。 这里是将method的方法指定给新建的这个btn。
```
## 按钮的样式

如果需要给按钮增加图片，则需要这么做：todo


# UISwitch

```objc
switchButton = [[UISwitch alloc] initWithFrame:CGRectMake(0, 0, 60, 30)];
[switchButton addTarget:self action:@selector(onSwitch:) forControlEvents:UIControlEventValueChanged];
    // 显示的颜色
switchButton.onTintColor = [UIColor colorWithRed:0.984 green:0.478 blue:0.224 alpha:1.000];
    // 控件大小，不能设置frame，只能用缩放比例
switchButton.transform = CGAffineTransformMakeScale(0.75, 0.75);

//开关事件
-(void)onSwitch:(id)sender
{
    UISwitch* switch=(UISwitch*)id;
    if(switch.value){
        NSlog(@"switch button has been opened");
    }
}
```


# UISegment

```objc
UISegmentedController *segment = [[UISegmentedControl alloc] initWithFrame:CGRectMake(150, 9, 120, 30)];
[segment insertSegmentWithTitle:@"东坡肉" atIndex:0 animated:YES]
[segment insertSegmentWithTitle:@"宫保鸡丁" atIndex:1 animated:YES];
[segment insertSegmentWithTitle:@"麻婆豆腐" atIndex:1 animated:YES];
segment.segmentedControlStyle = UISegmentedControlStyleBordered;
segment.momentary = NO;//这里如果设置成YES的话，选中一个分段时不会一直显示蓝色
segment.multipleTouchEnabled = NO;
[segment addTarget:self action:@selector(segmentAction:) forControlEvents:UIControlEventValueChanged];


//事件
-(void)segmentAction:(id)sender
{
    NSLog(@"you select item=%d",[sender selectedSegmentIndex]);
}
```

# UISlider


# UITextField
文本输入框，和label比较增加了文本输入和处理功能。

## 基本用法
```objc
UITextField *textUserName = [[UITextField alloc]initWithFrame:CGRectMake(20, 20, 130, 30)];
//设置边框样式，只有设置了才会显示边框样式
textUserName.borderStyle = UITextBorderStyleRoundedRect;
//设置输入框的背景颜色，此时设置为白色 如果使用了自定义的背景图片边框会被忽略掉
textUserName.backgroundColor = [UIColor whiteColor];
//设置背景图片
textUserName.background = [UIImage imageNamed:@"dd.png"];
//设置被禁用时的背景
textUserName.disabledBackground = [UIImage imageNamed:@"cc.png"];
//当输入框没有内容时，水印提示 提示内容为password
textUserName.placeholder = @"mailbox";
//设置输入框内容的字体样式和大小
textUserName.font = [UIFont fontWithName:@"Arial" size:20.0f];
text.textColor = [UIColor redColor];
text.text = @"一开始就在输入框的文字";
text.secureTextEntry = YES;//是否按照密码显示
```
## 相关事件

```objc 
//添加事件的格式
[textField addTarget:self action:@selector(changedTextField:) forControlEvents:UIControlEventEditingChanged];

//值变化事件
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
   NSLog(@"textField4 - 正在编辑, 当前输入框内容为: %@",textField.text);
   return YES;
}
//UIControlEventEditingChanged文本改变事件，每次文本修改都会调用。
-(void)changedTextField:(id)textField
{
    NSLog(@"值是---%@",textField.text); 
}
//用户按下return的事件，表示文本编辑确认
-(BOOL)textFieldShouldEndEditing:(UITextField *)textField
{
    str = textField.text;

    return YES;
}
```

## 扩展用法
```objc
//边框样式
text.borderStyle = UITextBorderStyleRoundedRect;
 typedef enum {
    UITextBorderStyleNone, 
    UITextBorderStyleLine,
    UITextBorderStyleBezel,
    UITextBorderStyleRoundedRect  

  } UITextBorderStyle;
 //输入框中是否有个叉号，在什么时候显示，用于一次性删除输入框中的内容
  text.clearButtonMode = UITextFieldViewModeAlways;
 
typedef enum {
    UITextFieldViewModeNever,  重不出现
    UITextFieldViewModeWhileEditing, 编辑时出现
    UITextFieldViewModeUnlessEditing,  除了编辑外都出现
    UITextFieldViewModeAlways   一直出现
} UITextFieldViewMode;
```