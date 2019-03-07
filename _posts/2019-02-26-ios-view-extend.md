---
layout: post
title: UIView扩展控件基本用法
categories: ios
description: UIImageView，UIScrollView，UITableView，UICollectionView等
keywords: ios objc
---


# UIView

##常见属性和方法
``` objc
//这几个属性都支持隐式动画的
@property(nonatomic) CGRect            frame;//view的相对于父控件的位置(x,y)和大小(width,height)
@property(nonatomic) CGRect            bounds; //view的相对于自身的位置(x,y)和大小(width,height)    (x,y)一般为(0,0)
@property(nonatomic) CGPoint           center;//view的中心点相对于父控件的位置
@property(nonatomic) CGAffineTransform transform; //view的形变属性

//添加一个view并设置红色背景色
UIView *view = [[UIView alloc] init];
view.backgroundColor = [UIColor redColor];
view.frame = CGRectMake(150, 300, 100, 100);
//设置旋转45度的形变属性
view.transform = CGAffineTransformRotate(view.transform, M_PI_4);
[self.view addSubview:view];
```
## 视图从属关系
``` objc
@property(nullable, nonatomic,readonly) UIView       *superview;//所属父类
@property(nonatomic,readonly,copy) NSArray<__kindof UIView *>*subviews;//所有的子类
@property(nullable, nonatomic,readonly) UIWindow     *window;//所属的window

- (void)removeFromSuperview;//从父类中移除
- (void)addSubview:(UIView *)view;//添加子类
- (nullable __kindof UIView *)viewWithTag:(NSInteger)tag;//通过tag搜索子view
```
layer层的使用请参考 CALayer的使用//todo 超链接


# UIImageView

## 基本用法
图片展示控件，负责展示图片。
```objc
//初始化
UIImageView  *imageView=[[UIImageView alloc] initWithFrame:CGRectMake(100, 200, 120, 120)];
//加载图片有2种方式，第一种从缓存加载，图片被保存到缓存
UIImage* image=[UIImage imageNamed:@"jpg1"]];
//第二种：从文件加载
NSString *filePath=[[NSBundle mainBundle] pathForResource:@"1" ofType:@"jpeg"];
UIImage *images=[UIImage imageWithContentsOfFile:filePath];
//设置图片
[imageView setImage:images]; 
//图片的拉伸,有四种风格
imageGroup.contentMode = UIViewContentModeScaleAspectFit;//

typedef NS_ENUM(NSInteger, UIViewContentMode) {
//图片拉伸填充至整个UIImageView(图片可能会变形),这也是默认的属性,如果什么都不设置就是它在起作用
    UIViewContentModeScaleToFill,
//图片拉伸至完全显示在UIImageView里面为止(图片不会变形)
    UIViewContentModeScaleAspectFit,
//图片拉伸至图片的的宽度或者高度等于UIImageView的宽度或者高度为止.看图片的宽高哪一边最接近UIImageView的宽高,一个属性相等后另一个就停止拉伸.
    UIViewContentModeScaleAspectFill,
//调用setNeedsDisplay 方法时,就会重新渲染图片
};
```
关于填充内容参考 UIViewContentMode详解<https://www.jianshu.com/p/8c784b59fe6a>
## 事件处理
```objc
//为图片添加点击事件
imageView.userInteractionEnabled = YES; 
UITapGestureRecognizer *singleTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(yourHandlingCode:)]; 
[imageView addGestureRecognizer:singleTap]; 
-(void)yourHandlingCode:(UIGestureRecognizer *)gestureRecognizer
{
     UIImageView *view = [gestureRecognizer view];
     int tagvalue = view.tag;
}
```

# UIScrollView
参考文档 UIScrollView——用法详解<https://www.jianshu.com/p/49b9e7ad493e>
UIScrollView是用来在屏幕上显示那些在有限区域内放不下的内容。例如，在手机屏幕上显示很大的图片。在这种情况下，需要用户对屏幕内容进行拖动来查看窗口区域外的内容。

## 内容展示：
contentSize
contentOffset
contentInset

contentSize
描述了有多大范围的内容需要使用scrollView的窗口来显示，其默认值为CGSizeZero，也就是一个宽和高都是0的范围。

contentOffset: 描述了内容视图相对于scrollView窗口的位置(当然是向上向左的偏移量咯)。默认值是CGPointZero，也就是(0,0)。当视图被拖动时，系统会不断修改该值。也可以通过

setContentOffset:animated:方法让图片到达某个指定的位置。

contentInset: 表示scrollView的内边距，也就是内容视图边缘和scrollView的边缘的留空距离，默认值是UIEdgeInsetsZero，也就是没间距。

bounces属性：描述的当scrollview的显示超过内容区域的边缘以及返回时，是否有弹性，默认值为YES。值为YES的时候，意味着到达contentSize所描绘的的边界的时候，拖动会产生弹性。值为No的时候，拖动到达边界时，会立即停止。
alwaysBounceHorizontal
：默认值为NO，如果该值设为YES，并且bounces也设置为YES，那么，即使设置的contentSize比scrollView的size小，那么也是可以拖动的。
alwaysBounceVertical
：默认值为NO，如果该值设为YES，并且bounces也设置为YES，那么，即使设置的contentSize比scrollView的size小，那么也是可以拖动的。

indicatorStyle：状态条显示风格，有下面几种
showsHorizontalScrollIndicator，
showsVerticalScrollIndicator
scrollIndicatorInsets
flashScrollIndicators

缩放：可以对显示内容进行缩放，类似看图是用滑动手势进行放大和缩小
maximumZoomScale: 最大放大比例，默认值为1，不得小于minimumZoomScale
minimumZoomScale: 最小放大比例，默认值为1，不得大于maxmumZoomScale
bouncesZoom: 描述在缩放超过缩放比例时，是否bounce，默认值为YES。如果值为NO，则达到最大或最小缩放比例时会立即停止缩放。否则，产生弹簧效果。
zoomScale: 当前的缩放比例。系统会根据缩放过程调整此值。
setZoomScale:animated:: 程序设置缩放大小。
zoomToRect:animated:将内容视图缩放到指定的Rect中。

## 其他属性：
scrollEnabled
:视图是否可被拖动，默认值为YES。一旦该值设置为NO，则scrollView不再接受触屏事件，会直接传递响应链。
scrollToTop
:是否启动“滚动至顶端”手势，默认值为YES。当用户使用了“滚动至顶端”手势(轻击状态栏)时，系统会要求离状态栏最近的scrollView滚动到顶端，如果scrollToTop设置为NO，则该scrollView的delegate的scrollViewShouldScrollToTop:
方法会返回NO，不会滚动到顶端。否则，则会滚动到顶端。滚动到顶端之后，会给delegate发送scrollViewDidScrollToTop:消息。需要注意的是，在iphone上，如果有多个scrollview的scrollToTop参数设置为YES的时候，“滚动至顶端”手势会失效。
delaysContentTouches
:是否推迟触屏手势处理，默认值为YES。设置为YES的时候，系统在确定是否发生scroll事件之后，才会处理触屏手势，否则，则会立即调用touchesShouldBegin:withEvent:inContentView:
方法。
directionalLockEnabled
:是否锁定某个特定方向的滚动，默认值为NO。设置为YES时，一旦用户向水平或竖直方向拽动时，另一个方向的滚动则被锁定了。但是如果首次拽动是斜着的，那么则不会锁定方向。
keyboardDismissMode
: 当拖动发生时，键盘的消失模式，默认值是不消失。
pagingEnabled
:是否使用分页机制，默认值为NO。当设置为YES时，会按照scrollView的宽度对内容视图进行分页。
decelerationRate
: 手指滑动后抬起，页面的减速速率。可以使用UIScrollViewDecelerationRateNormal
和UIScrollViewDecelerationRateFast
常量值来设置合理的减速速率。


# UITableView
列表框，包括view框，段落section，和单元格cell
参考
## 基本说明
派生自UIScrollView，增加了内容显示
```objc
//初始化
devicesView=[[UITableView alloc]initWithFrame:CGRectMake(0, 0, 10, 20) style:UITableViewStylePlain];
//风格设置同uiscrollview，略。
devicesView.dataSource=self;//
devicesView.delegate=self;
```
style有下面几种，创建时设置，不可改变。
UITableViewStylePlain:默认风格，平面
UITableViewStyleGrouped：section组风格，允许数据分组。
详细区别在这里<https://www.jianshu.com/p/764ed5aa46cf>

## 数据源和代理
包括UITableViewDataSource和UITableViewDelegate，常用的如下：
```objc
//返回当前section的数目。
- (NSInteger)numberOfSectionsInTableView:(UITableView*)tableView
//返回当前section里的行数目。
- (NSInteger)tableView:(UITableView*)tableView numberOfRowsInSection:(NSInteger)section{
    return 1;
}
//用于设定cell的高度
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 40;
}
//根据行列创建cell。
- (UITableViewCell*)tableView:(UITableView*)tableView cellForRowAtIndexPath:(NSIndexPath*)indexPath{
    // 创建常量标识符
    static NSString *identifier = @"cellForDevice";
    // 从重用队列里查找可重用的cell
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier];
    // 判断如果没有可以重用的cell，创建
    if (cell==nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:identifier];
    }
    //cell继承自uiview，可以对cell增加子view。
    [MyCSS SetTableCellStyle:cell];
    [cell.imageView setImage:[UIImage imageNamed:@"ble"]];
    cell.textLabel.text = devicesList[indexPath.row];
    cell.detailTextLabel.text = @"binded";
}

//当选中Cell时候调用的方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{

}

//给tableview头的Cell和Model的数据交互
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
//定义tableView的头部的高度
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
//给tableview尾部的Cell和Model的数据交互
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
//定义tableView的尾部的高度
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section

```
## 自定义cell
自定义cell有2种，一种是在已有的cell上面增加一些组件，另一种是完全自定义。

### 给cell增加自定义组件
cell的组件包括图片，标题和详细内容，如果需要增加一个
```objc
    //增加自定义图片
    float wide=cell.contentView.bounds.size.width;
    UIImageView* imageArrow=[[UIImageView alloc]initWithFrame:CGRectMake(wide-50,0,40,40)];
    [imageArrow setImage:[UIImage imageNamed:@"power_on"]];
    [cell.contentView addSubview:imageArrow];
```
### 自定义新的cell
如果需要自定义cell，需要派生自UITableviewCell。
```objc
@interface RootTableViewCell : UITableViewCell
// 联系人头像
@property (nonatomic, strong) UIImageView *headerImageView;
// 联系人姓名label
@property (nonatomic, strong) UILabel *nameLabel;
// 电话号码的label
@property (nonatomic, strong) UILabel *phoneLabel;
@end

@implementation RootTableViewCell
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    self = [super initWithStyle:style reuseIdentifier:reuseIdentifier];
    if (self) {
        // 初始化子视图
        [self initLayout];
    }
    return self;
}
- (void)initLayout
{
    // 1.头像
    self.headerImageView = [[UIImageView alloc] initWithFrame:CGRectMake(16, 10, 100, 100)];
//    self.headerImageView.backgroundColor = [UIColor orangeColor];
    self.headerImageView.layer.masksToBounds = YES;
    self.headerImageView.layer.cornerRadius = 50;
    // cell提供了一个contentView的属性，专门用来自定义cell,防止在cell布局的时候发生布局混乱，如果是自定义cell，记得将子控件添加到ContentView上
    [self.contentView addSubview:self.headerImageView];
    
    // 2.姓名
    self.nameLabel = [[UILabel alloc] initWithFrame:CGRectMake(CGRectGetMaxX(self.headerImageView.frame) + 10, CGRectGetMinY(self.headerImageView.frame), 100, 40)];
//    self.nameLabel.backgroundColor = [UIColor redColor];
    [self.contentView addSubview:self.nameLabel];
    
    // 3.电话号码
    self.phoneLabel = [[UILabel alloc] initWithFrame:CGRectMake(CGRectGetMinX(self.nameLabel.frame), CGRectGetMaxY(self.nameLabel.frame) + 20, 200, 40)];
//    self.phoneLabel.backgroundColor = [UIColor greenColor];
    [self.contentView addSubview:self.phoneLabel];
    
}

初始化后提供了contectView，可以往里面添加对应的设备。
可以使用工厂模式，根据model的内容创建动态的cell。
controller中，使用自定义cell和基本的UITableViewCell相同。
```

# UICollectionView
和UITableView相似，不过是二维的，在左右和上下可以配置。
参考文章 [UICollectionView入门--使用系统UICollectionViewFlowLayout布局类](https://blog.csdn.net/meegomeego/article/details/16953489)

## 初始化

```objc
//初始化布局类(UICollectionViewLayout的子类)
UICollectionViewFlowLayout *fl = [[UICollectionViewFlowLayout alloc]init];

//初始化collectionView
self.collectionView = [[UICollectionView alloc]initWithFrame:CGRectZero collectionViewLayout:fl];

//设置代理
self.collectionView.delegate = self;
self.collectionView.dataSource = self;
```
需要实现的协议:
UICollectionViewDataSource, UICollectionViewDelegateFlowLayout
## UICollectionViewDataSource

/每一组有多少个cell
- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section;

//定义并返回每个cell
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath;

//collectionView里有多少个组
- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView;

//定义并返回每个headerView或footerView
- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath;

上面这个方法使用时必须要注意的一点是，如果布局没有为headerView或footerView设置size的话(默认size为CGSizeZero)，则该方法不会被调用。所以如果需要显示header或footer，需要手动设置size。
可以通过设置UICollectionViewFlowLayout的headerReferenceSize和footerReferenceSize属性来全局控制size。或者通过重载以下代理方法来分别设置
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForHeaderInSection:(NSInteger)section;
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForFooterInSection:(NSInteger)section;

## UICollectionViewDelegateFlowLayout
//每一个cell的大小
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath;

//设置每组的cell的边界, 具体看下图
- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section;

![Alt text](https://localhost:4000/_site/images/posts/ios/CollectionViewCell.png) 

//cell的最小行间距
- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section;

//cell的最小列间距
- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section;

//cell被选择时被调用
- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath;

//cell反选时被调用(多选时才生效)
- (void)collectionView:(UICollectionView *)collectionView didDeselectItemAtIndexPath:(NSIndexPath *)indexPath;