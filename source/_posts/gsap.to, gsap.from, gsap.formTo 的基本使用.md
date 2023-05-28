---
title: gsap.to, gsap.from, gsap.formTo 的基本使用
cover: https://images.pexels.com/photos/268533/pexels-photo-268533.jpeg
date: 2022-09-24 12:00:00
updated: 2022-09-24 12:00:00
---
## 1. gsap.to(targets, vars)
生成从初始位置（或状态）到目标位置（或状态）的动画
targets： 产生动画的对象
vars： 目标状态参数
```javascript
//从当前的位置移动到100的位置
gsap.to("div", { duration: 5, x: 100 });
```

## 2. gsap.from(targets, vars)
生成从设置位置（或状态）到初始位置（或状态）的动画
targets： 产生动画的对象
vars： 设置状态参数
```javascript
//从100的位置移动到初始位置
gsap.from("div", { duration: 5, x: 100 });
```

## 3. gsap.fromTo(targets, vars, vars)
生成从开始位置（或状态）到结束位置（或状态）的动画
targets： 产生动画的对象
vars（第1个）： 开始状态参数
vars(第2个)： 结束状态参数
```javascript
//从100的位置移动到300的位置
gsap.fromTo("div", { x: 100 },{ duration: 5, x: 300 });
```

## 4. targets
设置动画的对象，可以是“.class”、“id”等选择器

## 5. vars 参数
##### duration
动画持续时间
##### ease
动画的运动方式。比如“elastic”(弹性)， “strong.inOut”(强烈的先快后慢)等，默认值：“power1.out”(渐缓)
##### id
为tween实例分配id（可选），以便之后使用gsap.getById()来获取该tween
##### lazy
当tween第一次渲染并读取其初始值时，GSAP将尝试延迟值的写入，直到当前的tick(更新循环)的最后再进行写入，这样可以提高性能。因为它避免了浏览器读/写/读/写的频繁操作，这种频繁的操作会极大的消耗性能并造成布局抖动。设置lazy:false便禁用这种延迟渲染，但是不建议禁用。默认值：true
##### onComplete
动画完成时调用的函数
##### onCompleteParams
传递给onComplete函数的参数数组
##### onRepeat
当动画重复时调用的函数。需要设置repeat（大于0）才会有效
##### onRepeatParams
传递给onRepeat 函数的参数数组
##### onReverseComplete
当动画从反方向再次达到开始位置时要调用的函数。需要设置reversed: true##### id
为tween实例分配id（可选），以便之后使用gsap.getById()来获取该tween
##### onReverseCompleteParams
传递给onReverseComplete函数的参数数组
##### onStart
动画开始时要调用的函数
##### onStartParams
传递给onStartParams函数的参数数组
##### onUpdate
每次动画更新时（每帧）调用的函数
##### onUpdateParams
传递给onUpdate函数的参数数组
##### overWrite
如果设置true, 该对象的其他动画（任何属性）就会被停止；如果设置auto，只会停止其他动画的相关属性动画；如果false，则不停止任何动画，最后呈现的动画效果是各个动画相互作用的结果。 默认值：false
##### paused
如果设置true,动画将在创建时立即暂停。默认值：false
##### repeat
设置动画重复的次数。 设置1为重复两次， 默认值：0。设置-1为无限重复
##### repeatDelay
每次动画重复之间等待的时间（秒）。默认值：0
##### repeatRefresh
设置true 会导致重复的动画失效，因为每次完成一次后，会重新更新其开始和结束值。如果属性使用的动态值（随机或函数），会得到特别的动画效果。默认值：false。
##### reversed
如果设置true，动画将调转方向超其开始方向移动。由于开始已经是初始位置，设置true后动画将显示为暂停。默认值：false
##### runBackwards
如果设置true, 动画将翻转其起始值和结束值，但对于ease不会反转。可以通过设置true将gsap.to 转换为gsap.from。默认值：false
##### stagger
如果是多个动画目标，可以通过设置类似stagger:0.1（每个目标动画开始之间间隔0.1秒）来错开每个动画。或可以设置参数对象来实现更高级的动画效果
##### startAt
定义任何属性的初始值（即便它们没有设置动画）
##### yoyo
如果设置true，则每隔一次的重复都将以反方向的方向运动，动画呈现来回运动的效果。单次运动无效，需要配合repeat使用。默认值：false
##### yoyoEase
允许改变yoyo过程中的ease。可以指定特定的ease（例如：“power2.in”），可以设置true来翻转原本的ease。默认值：false。 另外：启动了该属性后，yoyo会自动设置true
##### keyframes
对同一对象的一连串的动画（关键帧动画）。和多个的gsap.to等效

## 6. vars 属性的Function类型参数
通过使用function类型参数来实现更复杂的动画效果
```javascript
//1. 每个div的运动的y值不同，按顺序依次多100。第一个div运动到{x:100, y:0}的位置，第2个div运动到{x:100,y:100}的位置，第3个div运动到{x:100,y:200}的位置...
gsap.to("div", { x: 100,
  y: function(index, target, targets){
    return index * 100;
  }
});

//2. 偶数索引的div沿y轴运动100，其他div则y保持不变
gsap.to("div", {
  x: 100,
  y: function(index, target, targets){
    return index % 2 === 0 ? 100 : 0;
  }
});
// 参数：
// index: 当前运动对象的索引
// target：当前运动对象
// targets: 全部运动对象的数组
```

