---
layout: post
title: Nave
categories: ios
description: UIImageView，UIScrollView，UITableView，UICollectionView等
keywords: ios objc
---

# 导航栏使用总结

参考文档 [导航栏使用总结](https://www.jianshu.com/p/50cd38f2772c)

## 创建和设置导航栏
在(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions 函数里添加下面的代码。
```objc

    //1.创建导航条
    _navController=[[UINavigationController alloc] init];
    [_navController pushViewController:self.window.rootViewController animated:YES];
    [self.window addSubview:_navController.view];
    //导航栏风格是全局的。
    //2.设置导航栏背景颜色
    [[UINavigationBar appearance] setBarTintColor:[UIColor orangeColor]];
        
    //3.设置导航栏背景图片
    [[UINavigationBar appearance] setBackgroundImage:[UIImage imageNamed:@"navigationBarImg"] forBarMetrics:UIBarMetricsDefault];
        
    //4.设置导航栏标题样式
    [[UINavigationBar appearance] setTitleTextAttributes: [NSDictionary dictionaryWithObjectsAndKeys: [UIColor purpleColor], NSForegroundColorAttributeName,
    [UIFont boldSystemFontOfSize:25], NSFontAttributeName, nil]];
        
    //5.设置导航栏返回按钮的颜色
    [[UINavigationBar appearance] setTintColor:[UIColor greenColor]];

    //6.设置导航栏隐藏
    [[UINavigationBar appearance] setHidden:YES];

```
## 配置导航栏的按钮
导航栏可以通过设置rightBarButtonItem和leftBarButtonItem来定义左边和右边的按钮。
```objc
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:nil style:UIBarButtonItemStylePlain target:self action:@selector(startSearching)];
    //设置图片，注意图片大小40*40，需要实现按尺寸缩放好。
    [self.navigationItem.rightBarButtonItem setImage:[UIImage imageNamed:@"setting"]];
```
# UITabBarController总结
参考文档 [UITableBar总结]https://www.jianshu.com/p/1b3e868df606

UITabBarController继承自UIViewController同时包含一个UITabBar和一个viewcontroller列表，可以通过UITabBar来控制激活对应的viewcontroller。
常用的方法包括创建和设置uitabbar。

## 创建
UITabBarController从UIViewController中继承，用作根视图控制器。
```objc
    UITabBarController * tabBar= [[UITabBarController alloc]init];
    NSMutableArray * controllerArray = [[NSMutableArray alloc]init];
    for (int i=0; i<4; i++) {
        UIViewController * con = [[UIViewController alloc]init];
        [con loadViewIfNeeded];
        con.view.backgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:1];
        con.tabBarItem.image = [UIImage imageNamed:@"btn_publish_face_a.png"];
        con.tabBarItem.title=[NSString stringWithFormat:@"%d",i+1];
        con.title = [NSString stringWithFormat:@"%d",i+1];
        [controllerArray addObject:con];
    }
    tabBar.viewControllers = controllerArray;
    [self presentViewController:tabBar animated:YES completion:nil];
```
## 定制TabBar基本风格
UITabBar包括一个背景view和若干个按钮（UITabBarItem），一个UITabBarDelegate用来响应用户选择按钮的额外动作处理。


## 定制UITabBarItem
UITabBarItem继承自UIBarItem。
UIBarItem包含图片和文字，可以设定图片和文字的位置，另外还可以设置文字大小等属性。
UITabBarItem增加了选择和未选择时的状态。

## 创建方法
- (instancetype)initWithTitle:(nullable NSString *)title image:(nullable UIImage *)image selectedImage:(nullable UIImage *)selectedImage
注意title选中后会变成viewcontroller的title。

### 显示设置
包括定制图片，文字的大小和颜色。
'''objc
    //调整图片尺寸大小，baritem需要30*30大小
    UIImage* image1=[self scaleToSize:[UIImage imageNamed:@"pause" ]  size:CGSizeMake(30, 30)];
    //设置图片为不渲染，否则会被替换颜色。（透明色不变，其他颜色变为tintcolor）
    UIImage* imageUnselected=[image1 imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    
    UIImage* image2=[self scaleToSize:[UIImage imageNamed:@"start"]  size:CGSizeMake(30, 30)];
    UIImage* imageSelected=[image2 imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    secondNavController.tabBarItem = [[UITabBarItem alloc] initWithTitle:@"speed" image:imageUnselected selectedImage:imageSelected];
    secondNavController.tabBarItem.imageInsets=UIEdgeInsetsZero;//设置边界，默认居中
    //更改tabbar下方的文字样式，大小， 颜色
    NSMutableDictionary *atts=[NSMutableDictionary dictionary];
    // 更改文字大小
    atts[NSFontAttributeName]=[UIFont systemFontOfSize:12];
    // 更改文字颜色
    atts[NSForegroundColorAttributeName]=[UIColor darkGrayColor];
    
    NSMutableDictionary *selectedAtts=[NSMutableDictionary dictionary];
    selectedAtts[NSFontAttributeName]=[UIFont systemFontOfSize:12];
    selectedAtts[NSForegroundColorAttributeName]=[UIColor greenColor];
    //修改单个item的。
    //[secondNavController.tabBarItem setTitleTextAttributes:selectedAtts forState:UIControlStateSelected];
    //如果需要一次修改所有的baritem，用这个。
    UITabBarItem *item=[UITabBarItem appearance];
    [item setTitleTextAttributes:atts forState:UIControlStateNormal];
    [item setTitleTextAttributes:selectedAtts forState:UIControlStateSelected];
'''