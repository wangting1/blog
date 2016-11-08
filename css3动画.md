# css3动画

### 基础介绍
- css3中动画主要用到animation，其用法如下：

```
animation:  name duration timing-function delay iteration-count direction fill-mode play-state;
```
- name:定义的动画名称。
- duration：动画执行所需时间
- timing-function：动画完成的速度。默认值为linear匀速，除此之外还有
	- ease:动画以低速开始，然后加快，在结束前变慢。
	- ease-in:以低速开始。
	- ease-out:以低速结束。
	- ease-in-out:以低速开始和结束。
	- cubic-bezier
- delay:动画开始的延时时间，注意这里允许负值，这里为负值时，动画会马上开始如－1，动画会马上开始且跳过第一秒的动画。
- iteration-count:动画播放的次数。infinite表示播放无数次。
- direction:多次执行动画时，动画是否循环反向播放。默认值：normal正常播放。
	- reverse:动画反向播放
	- alternate:奇数次正向播放，偶数次反向播放
	- alternate-reverse:偶数次正向播放，奇数次反向播放
- fill-mode:把目标元素从一个地方移动到另一个地方，并让它停留在那里
	- none:默认值
	- forwards:动画结束后，元素将保持在结束时的位置
	- backwards:动画将应用在 animation-delay 定义期间启动动画的第一次迭代的关键帧中定义的属性值。
	- both:动画遵循 forwards 和 backwards 的规则。也就是说，动画会在两个方向上扩展动画属性。

- play-state:指定动画是否正在运行或已暂停。
	- paused:暂停动画
	- running:运行动画
	
### fill-mode
在上述属性中fill-mode中的backwards和both是相对比较难理解的，最后终于在张鑫旭的博客中发现了相对还能看的解释，引用过来：

> backwards，返回，表示动画开始之前，元素处于keyframe是"from"或"0%"关键帧的状态。由于大多数动画的animatin-delay为0, 由于没有指定forwards状态(如：both值)，因此我们视觉上看到的表现是：动画结束后，动画回到了起始关键帧状态；实际是动画开始之前就如此，而不是结束，万万不可被此假象误导。
要想看清现实，可以设置animation-delay为非0值，我们就可以看到动画开始之前，元素就是"0%"起始关键帧状态，而不是元素默认状态。直观且准确反映了backwards的准确释义。

> 实际应用中，animation-delay设置了非0值，同时不是step-start动画形式，此参数慎用，除非元素默认状态就是起始帧状态，否则动画犹如抽风了一样~。
  
  <br />

> both，forwards和backwards双P, 这是个略考智商的属性，既当爹又当妈的，这可怎么搞！好搞的，如果要求同一时间既爹又妈，你不是人妖，搞不来。但是白天当爹，晚上当妈，我想相对容易多。这里也是如此，both是与的关系，中文意思是“同时”，表示：动画开始之前是"from"或"0%"关键帧；动画完成之后是"to"或"100%"关键帧状态。

```
		@keyframes animations {
            0% {
                transform: translate(0, 0);
            }
            100% {
                transform: translate(400px);
            }
        }        
        .none span {
            -webkit-animation: animations 4s ease;
            animation: animations 4s ease;
        }
        .forwards span {
            animation: animations 4s ease forwards;
        }
                
        
        
        @keyframes animations3 {
            0% {
                transform: translate(100px, 0);
            }
            100% {
                transform: translate(400px);
            }
        }
        .backwards span {
            -webkit-animation: animations3 4s ease 2s backwards;
            animation: animations3 4s ease 2s backwards;
        }
        
        .both span {
            -webkit-animation: animations3 4s ease 2s both;
            animation: animations3 4s ease 2s both;
        }
        
```

上述css运行后，由于backwards和both的动画时延迟2s之后执行的，发现在动画开始前backwards和both的状态是0%时transform: translate(100px, 0)的样式，而none和forwards已经开始运动。

由此可以看出backwards和both属性在动画开始之前，元素就是"0%"起始关键帧状态，而不是元素默认状态。

![动画开始2s以内的样式](http://s17.mogucdn.com/new1/v1/bmisc/af6319f7e4becca4b233416d52bf82c4/189416858329.jpg)

动画执行完毕后样式如下图，可以看出forwards和both属性的元素在动画完成之后是"to"或"100%"关键帧状态。

![动画结束后的样式](http://s17.mogucdn.com/new1/v1/bmisc/05f82c89cb48efa20dc96dc3f6ce9bca/189417073271.jpg)

*总结：forwards影响动画结束时元素的状态，让元素保持"to"或"100%"关键帧状态。backwards在有动画延时时，让元素在动画延时这段时间就保持"0%"起始关键帧状态，而不是元素默认状态。both则是两者兼具。*

