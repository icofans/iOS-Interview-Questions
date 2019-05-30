# 动画
## 1.UIView动画与核心动画的区别? 

- 核心动画只作用在layer. 
- 核心动画修改的值都是假像.它的真实位置没有发生变化.
- 当需要与用户进行交互时用UIView动画,不需要与用户进行交互时两个都可以.

## 2.当我们要做一些基于 CALayer 的动画时，有时需要设置 layer 的锚点来配合动画，这时候我们需要注意什么？

- 需要注意的是设置锚点会引起原来 position 的变化，可能会发生不符合预期的行为，所以要做一下转化，示例代码如下：
	
	``` c 
	// 为 layer 的动画设置不同的 anchor point，但是又不想改变 view 原来的 position，则需要做一些转换。
    - (void)setAnchorPoint:(CGPoint)anchorPoint forView:(UIView *)view {
        // 分别计算原来锚点和将更新的锚点对应的坐标点，这些坐标点是相对该 view 内部坐标系的。
        CGPoint oldPoint = CGPointMake(view.bounds.size.width * view.layer.anchorPoint.x,
                                       view.bounds.size.height * view.layer.anchorPoint.y);
        CGPoint newPoint = CGPointMake(view.bounds.size.width * anchorPoint.x,
                                       view.bounds.size.height * anchorPoint.y);
        
        // 如果当前 view 有做过 transform，这里要同步计算。
        oldPoint = CGPointApplyAffineTransform(oldPoint, view.transform);
        newPoint = CGPointApplyAffineTransform(newPoint, view.transform);
        
        // position 是当前 view 的 anchor point 在其父 view 的位置。
        CGPoint position = view.layer.position;
        // anchor point 的改变会造成 position 的改变，从而影响 view 在其父 view 的位置，这里把这个位移给计算回来。
        position.x = position.x + newPoint.x - oldPoint.x;
        position.y = position.y + newPoint.y - oldPoint.y;
        
        view.translatesAutoresizingMaskIntoConstraints = YES;
        view.layer.anchorPoint = anchorPoint; // 设置了新的 anchor point 会改变位置。
        view.layer.position = position; // 通过在 position 上做逆向偏移，把位置给移回来。
    }
    ```