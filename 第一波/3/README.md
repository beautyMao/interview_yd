## 随便打开一个网页，用 JavaScript 打印所有以 s 和 h 开头的标签，并计算出标签的种类

```javascript
// 转换为真正的数组
let el = Array.from(document.getElementByTagName("*"))
let elOjb = {}
//正则判断是否是h/s开头
let reg = /^[h|s].+/gi
el.map((item) => {
  const tagName = item.tagName
  if (reg.test(tagName)) {
    !elObj[tagName] ? (elObj[tagName] = 1) : elObj[tagName]++
  }
})
```

## 给定一个数组，按找到每个元素右侧第一个比它大的数字，没有的话返回-1 规则返回一个数组

给定数组：[2,6,3,8,10,9]返回数组：[6,8,8,10,-1,-1]

1)暴力双重循环

```javascript
function findMaxRight(arr) {
  let result = []
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] > arr[i]) {
        result[i] = result[j]
        break
      } else {
        result[i] = -1
      }
    }
  }
  result[result.length] = -1
  return result
}
```

2)利用栈的特性

```javascript
function findMaxRightWithStack(arr) {
  const size = arr.length
  let indexArr = [0]
  let result = []
  let index = 1
  while (index < size) {
    if (indexArr.length > 0 && arr[indexArr[indexArr.length - 1]]) {
      result[indexArr[indexArr.length - 1]] = array[index]
      indexArr.pop()
    } else {
      indexArr.push(index)
      index++
    }
  }
  indexArr.forEach((item) => {
    result[item] = -1
  })
  return result
}
```

3)单调递减栈，反向遍历

```javascript
function firstBiggerItem(T) {
  const res = new Array(T.length).fill(-1)
  const stack = []
  for (let i = T.length - 1; i >= 0; i--) {
    while (stack.length && T[i] >= T[stack[stack.length - 1]]) {
      stack.pop()
    }
    if (stack.length && T[i] < T[stack[stack.length - 1]]) {
      res[i] = T[stack[stack.length - 1]]
    }
    tsck.push(i)
  }
  return res
}
```

## Https 和 http 的区别是什么？https 为什么比 http 安全？如何进行配置 Https？HTTPS 是怎么建立安全通道的？介绍下 Https 的加密过程和获取加密秘钥的过程？301、302 的 https 被挟持怎么办?

#### https 和 http 的区别

1. https 协议需要到 CA 申请证书,一般免费证书较少，因而需要一定费用。
2. http 是超文本传输协议,信息是明文传输,https 则是具有安全性的 SS/tls 加密传输协议。
3. http 和 https 使用的是完全不同的连接方式,用的端口也不ー样,前者是 80,后者是 443。
4. http 的连接很简单,是无状态的；HTTPS 协议是由 SSL/TLS+HTTP 协议构建的可进行加密传输、身份认证的网络协议,比 http 协议安全。

#### https 为什么比 http 安全？

1. 在交换密钥环节使用非对称加密方式,防止秘钥被非法获取;建立通信后,交换报文阶段则使用对称加密方式,防止报文被窃听
2. 通过数字签名解決了报文可能被簒改问题数字签名有两种功效:
   > 能确定消息确实是由发送方签名并发出来的,因为另人假冒不了发送方的签名。
   > 数字签名能确定消息的完整性,证明数据是否未被簒改过。
3. 通过数字证书解决了通信方身份可能被伪装的问题

#### 如何进行配置

tomcat 部署

1. 申请证书
   证书由第三方 CA 认证机构颁发,网站所有者向 CA 机构申请证书,证书中包含了公钥、颁证机构、网址、失效日期。如果网站使用假冒证书,浏览器向 CA 认证机构发送证书是否合法的请求,如果检测非法证书,浏览器可以直接断开请求.
2. 证书部暑到 Tomcat
   配置 SSL 连接器,将
   需浏览器打开:W.domain.com.jks 文件存放到 conf 目录下,然后配置同目录下的 Server.xm 文件
   不全

#### 介绍下 Https 的加密过程和获取加密秘钥的过程

HTTPSS 建立安全通道过程及 Https 的加密过程和获取加密秘钥的过程,在 https 使用对称密钥发送报文时,安全通道已建好

1. Client 发起一个 HIPS(
   需浏览器打开:https://www.baidu.com/)的请求,根据 RFC2818 的规定, Client 知道需要连接 Servere 的 443(默认)端口。

2. Server 把事先配置好的公钥证书( public keycertifcate)返回给客户端。
3. Client 验证公钥证书:比如是否在有效期内,证书的用途是不是匹配 Client 请求的站点,是不是在 CRL 吊销列表里面,它的上一级证书是否有效,这是一个递归的过程,直到验证到根证书(操作系统内置的 Root 证书或者 lientp 内置的 Root 证书)。如果验证通过则继续,不通过则显示警告信息。
4. Client1 使用伪随机数生成器生成加密所使用的对称密钥,然后用证书的公钥加密这个对称密钥,发给 Server。
5. Servert 使用自己的私钥( private key)解密这个消息,得到对称密钥。至此, Client:和 Server 双方都持有了相同的对称密钥。
6. Servert 使用对称密钥加密“明文内容 A”,发送给 Client。
7. Client 使用对称密钥解密响应的密文,得到“明文内容 A”
8. Client 再次发起 HTPS 的请求,使用对称密钥加密请求的“明文内容 B”,然后 Servert 使用对称密钥解密密文,得到“明文内容 B”。

#### 301、302 的 https 被挟持怎么办

> 合理使用 HSTS
> 什么是 HSTS
> HSTS(HTTPStrictTransportSecurity,HTTP 严格传输安全协议)表明网站已经实现了 TLS,要求浏览器对用户明文访问的 UrI 重写成 HTTPS,避免了始终强制 302 重定向的延时开销。
> HSTS 的实现原理
> 当浏览器第一次 HTTP 请求服务器时,返回的响应头中增加 Strict- Transport- Security,告诉浏览器在指定的时间内,这个网站必须通过 HTTPS 协议来访问。

也就是对于这个网站的 HTTP 地址,浏览器需要现在本地替換为 HTPS 之后再发送请求。

## 谈一下微信小程序的架构以及为什么要用到双线程

## 请实现如下的函数

可以批量请求数据，所有的 URL 地址在 urls 参数中，同时可以通过 max 参数控制请求的并发度，当所有请求结束之后，需要执行 callback 回调函数。发请求的函数可以直接使用 fetch 即可

## 1000\*1000 的画布，上面有飞机、子弹，如何划分区域能够更有效的做碰撞检测，类似划分区域大小与碰撞检测效率的算法，说一下大致的思路

## tcp 和 udp 有什么区别？tcp 怎样确保数据正确性？tcp 头包含什么？tcp 属于哪一层？详细介绍下 TCP 三次握手

## 说一下对面向对象的理解及其好处？面向对象的三要素是啥？

## 说一下对 Redux 的理解并介绍下 Redux 的原理？Redux 和 Vuex 有什么区别，说下一它们的共同思想？ 它的设计思想及工作流程是怎样的？主要解决什么问题？

## 为什么 useState 要使用数组而不是对象

## 讲一下 import 的原理，与 require 有什么不同？如何实现按需加载

## 详细说一下 GC？另外 JavaScript 里垃圾回收机制是什么？常用的是哪种？怎么处理的？

一、常用的垃圾回收机制

1. 引用计数法 reference counting

1) 跟踪记录每个值被引用的次数
2) 当声明变量并将一个引用类型的值赋值给该变量时,则这个值的引用次数加 1
3) 同一值被赋予另一个变量,该值的引用计数加 1
4) 当引用该值的变量被另一个值所取代,则引用计数减 1,
5) 当计数为 O 的时候,说明无法在访这个值了,系统将会收回该值所占用的内存空间。
6) 缺点:循环引用的时候,引用次数不为 0,不会被释放;

2. 标记清除法 mark- sweep
   给存储在内存中的变量都加上标记,判断哪些变量没有在执行环境中引用,进行删除;
3. 停止复制 stop-copy
   内存分为两块,正在使用的存活对象复制到未被使用的内存块中,清除正在使用的内存块中的所有对象,然后交换内存的角色
4. 标记压缩 mark- compact
   所有可达对象做一次标记,所有存活对象压缩到内存的一端,減少内存碎片
5. 増量算法 incremental collecting
   每次 GC 线程只收集一小片内存空间,接着切换到应用程序线程,依次反复,直到垃圾收集完成

二、堆、栈的数据的 GC
js 的内存空间有调用栈 stack 和堆空间 heap

1. 调用栈中的数据的 GC
   调用栈有一个记录当前执行状态的指针(称为 ESP),JS 引擎通过下移 ESP 指针来销毁栈顶某个执行上下文,释放栈空间
2. 堆空间中数据的 GC
   分代收集:新生代、老生代
   提高垃圾回收的效率,V8 将堆分为新生代和老生代两个部分

o 其中新生代为存活时间较短的对象(需要经常进行垃圾回收),内存占用小,GC 频繁;只支持 1-8M 容量;,使用副垃圾回收器
o 而老生代为存活时间较长的对象(垃圾回收的频率较低),内存占用多,GC 不频繁;使用主垃圾回收器;

##### 新生代的 GC 算法

新生代的对象通过 Scavenge 算法进行 GC。在 Scavenge 的具体实现中,主要采用了 Cheney 算法。

1. Cheney 算法是一种采用停止复制(stop-copy)的方式实现的垃圾回收算法。
2. 它将堆内存一分为二,每一部分空间成为 semispace-半空间。
3. 在这两个 semispace 空间中,只有一个处于使用中,另一个处于闲置中。
4. 处于使用中的 Semispace 空间成为 From 对象空间,处于闲置状态的空间成为 To 空闲空间。
5. 当我们分配对象时早在 rem 间中进行分配。当开始进行垃圾回收时,会检查 From 空间中的存活对象,这些存活对象将被复制到 To 空间中,同时还会将这些对象有序的排列起来~~相当于内存整理,所以没有内存碎片,而非存活对象占用的空间将被释放。完成复制后,From 空间和 To 空间的角色发生对換。
6. Scavenge 是典型的空间换取时间的算法,而且复制需要时间成本,无法大规模地应用到所有的垃圾回收中,但非常适合应用在新生代中进行快速频繁清理。

##### 对象晋升策略

对象从新生代中移动到老生代中的过程称为晋升。
晋升条件主要有两个
o 对象是否经历过两次 Scavenge 回收都未清除,则移动到老生代
o 空间已经使用超过 25%,To 空间对象移动到老生代
因为这次 Scavenge 回收完成后,这个 To 空间将变成 From 空间,接下来的内存分配将在这个空间中进行,如果占比过高,会影响后续的内存分配

##### 写屏障

写缓冲区中有一个列表( Crossreflist),列表中记录了所有老生区对象指向新生区的情况
这样可以快速找到指向新生代该对象的老生代对象,根据他是否活跃,来清理这个新生代对象;

##### 老生代的 GC

老生代的内存空间较大且存活对象较多,使用新生代的 Scavenge 复制算法,会耗费很多时间,效率不高;而且还会浪费一半的空间;
为此 V8 使用了标记一清除算法(Mark- Sweep)进行垃圾回收,并使用标记-压缩算法(Mark- Compact)整理内存碎片,提高内存的利用率。步骤如下:

1. 对老生代进行第一遍扫描,标记存活的对象,从一组根元素开始,递归遍历这组根元素,在这个遍历过程中,能到达的元素称为活动对象,没有到达的元素就可以判断为垃圾数据;
2. 对老生代进行第二次扫描,清除未被标记的对象
3. 标记-整理算法,标记阶段一样是递归遍历元素,整理阶段是将存活对象往内存的一端移动
4. 清除掉存活对象边界外的内存
   > 注意,不管那种,整理内存后只要地址有变化,需要及时更新到调用栈的

##### 全停顿 Stop-The-World

Javascript 是运行在主线程之上的,一旦执行垃圾回收算法,都需要将正在执行的 Javascript 脚本暂停下来,待垃圾回收完毕后再恢复脚本执行。我们把这种行为叫做全停顿 Stop-the-world)
新生代因为内存小,活动对象少,全停顿影响不大,但是老生代可能会造成卡顿明显;
解决办法:增量标记算法 ncremental Marking  
V8 将标记过程分为ー个个的子标记过程,同时让垃圾回收标记和 Javascript 应用逻辑交替进行,直到标记阶段完成

##### Javascrip 中的垃圾回收

V8( Javascript 引擎)的新老空间内存分配与大小限制

##### 新老空间

凡事都有一把双刀剑,在垃圾回收的演变过程中人们发现,没有一种特定的垃圾回收机制是可以完美的解决问题,因此 V8 采用了新生代与老生代结合的垃圾回收方式,将内存分为新生代和老生代。新生代频繁进行 GC,空间小,采用的是空间換时间的 scavenge 算法,所以又划分为两块 semispace,From 和 To。老生代大部分保存的是存活时间较长的或者较大的对象。采用的是 mark- sweep(主)&mark- compact(辅)算法。
V8 限制了 js 对象可以使用的内存空间,不止是因为最初 V8 是作为浏览器引擎而设计的。还有其垃圾回收机制的影响因素。V8 使用 stop-the-Word(全停顿), generationalaccurate 的垃圾回收器。在执行回收之时会暂时中断程序的执行,而且只处理对象堆栈。当内存达到一定的体积时,进行一次垃圾回收的时间将会很长,从而影响其相应而造成汭览器假死的状况。因此,在 V8 中限制老生代 64 位为 1.4GB,32 位为 0.7GB,新生代 64 位为 32M,32 位为 16M。当然,如果需要更大的内存空间,在 node 中可以进行更改

##### 对象晋升

新生成的对象放入新生代内存中,那哪些对象会被放入老生代中呢?大部分放入老生代的对象是由新生代晋升而来。对象的晋升的方式:

1. 当新生代的 To semispace 内存占满 25%时,此时再人从 Fromsemispace 拷贝对象将不会再放入 To 空间中以防影响后续的新对象分配,而将其直接复制到老生代空间中。
2. 在进行一次垃圾回收后,第二次 GC 时,发现已经经历过次 GC 的对象在从 From 空间复制时直接复制到老生代。
3. 在新对象分配时大部分对象被分配到新生代的 Fromsemispace,但当这个对象的体积过大,超过 1MB 的内存页时,直接分配到老生代由 larcya-alhjoat Space。

##### 新生代的 GC 机制与优缺点

##### 回收机制

新生代采用 Scavenge 算法,在 scavenge 算法的实现过程中,则主要采用了 cheney 算法。即使用复制方式来实现垃圾回收。它将内存一分为二,每一个空间都是一个 Semispaceo。
处于使用状态的是 From 空间,闲置的是 To 空间。当分配对象时,先是分配到 From 空间,垃圾回收时会检查 From 空间中存活的对象,将其复制到 To 空间,回收其他的对象。完成复制后会进行紧缩,From 和 To 空间的调换。如此循环往复。

##### 优势

由其执行的算法及过程我们可以了解到,在新生代的垃圾回收过程中,总是由一半的 Semispace 是空余的。 Scavenge 只复制存活的对象,在新生代的内存中,存活的对象相对较,所以使用这个算法恰到好处.

##### 老生代的 GC 机制与优缺点

##### 回收机制

由于的 scavenge 算法只复制存活的对象,如果在老生代中也使用此算法的话就会造成复制很多对象,效率低,并且造成很大的内存空间浪费。老生代中采用的则是 mark-Sweep(标记清除)和 mark- compact(标记整理)结合的方式。而为什么使用两者结合呢?这就要讲到两者的优点与缺点

###### mark- sweep(标记清除)

1. 优点
   o 标记清除需要标记堆内存中的所有对象,标记出在使用的对象,清除那些没有被标记的对象。在老生代内存中与新生代相反,不使用的对象只占很小一部分,所以清除不用的对象效率高。
   o mark- sweep 不会将内存空间分为两半,所以,不会浪费一半空间。
2. 缺点
   但标记清除会造成一个问题,就是在清除过后会导致内存不连续,造成内存碎片,如果此时需要储存一个很大的内存而空间又不够的时候就会造成没有必要的反复垃圾回收。

###### mark- compact(标记整理)

1. 优点
   此时标记整理就可以出场了,在标记清除的过程中,标记整理会将存活的对象和需要清除的对象移动到两端。然后将其中一段需要清除的消灭掉,可以解決标记清除造成的内存碎片问题。
2. 缺点
   但是在紧缩内存的过程中需要移动对象,效率比较低。所以 V8 在清理时主要会使用 Mark- Sweep.,在空间不足以对新生代中晋升过来的对象进行分配时才会使用 Mark- compact。

##### 垃圾回收机制的优化

增量标记在老空间里引入了此方式
scavenge 算法,mark- sweep 及 mark- compact 都会导致 stop-the-orld(全停顿)。而全停顿很容易帯来明显的程序迟滞,标记阶段很容易就会超过 100ms,因止此 V8 引入了增量标记,将标记阶段分为若干小步骤,每个步骤控制在 5ms 内,每运行一段时间标记动作,就让 Javascript 程序执行一会儿,如此交替,明显地提高了程序流畅性,一定程度上避免了长时间卡顿。

## Promise 有没有解决异步的问题?promise 通过 catch 捕获到 reject 之后，在 catch 后面还能继续执行 then 方法嘛，如果能执行执行的是第几个回调函数?promise 里面和 then 里面执行有什么区别?Promise 构造函数是同步还是异步执行，then 呢？

## 类设计：使用面相对象设计一个停车场管理系

题目要求
使用面相对象设计一个停车场管理系统，该停车场包含：

- 1.停车位，用于停放车辆；
- 2.停车位提示牌，用于展示剩余停车位；
_可以丰富该系统的元素，给出类，类属性，类接口。
_/
15 详细说一下浏览器解析 Html 文件的过程？
16 是否了解 glob，glob 是如何处理文件的，业界是否还有其它解决方案
17 浏览器都有哪些进程，渲染进程中都有什么线程
说说浏览器渲染流程，分层之后在什么时候合成，什么是重排、重绘，怎样避免？
什么情况会出现浏览器分层
18 怎样判断一个对象是否是数组，如何处理类数组对象
19 手写 dom 操作，翻转 li 标签，如何处理更优
/\*
_有下边这样的 dom 结构，现在可以获取到 ul，要求翻转里边 li 标签，如何处理更优
_/
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
20	什么是同源策略？什么是跨域？都有哪些方式会造成跨域？解决跨域都有什么手段？
21	单向链表实现队列
22	es5 实现 isInteger
23	说一下 Vue 的 keep-alive 是如何实现的，具体缓存的是什么？
24	对称加密和非对称加密的区别和用处
25	BFC 是什么？触发 BFC 的条件是什么？有哪些应用场景？
26	encoding 头都有哪些编码方式? 
utf-8 和 asc 码有什么区别?
说一下 base64 的编码方式
27	实现输出一个十六进制的随机颜色(#af0128a)
28	通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？
29	移动设备安卓与 iOS 的软键盘弹出的处理方式有什么不同，iPhone 里面 Safari 上如果一个输入框 fixed 绝对定位在底部，当软键盘弹出的时候会有什么问题，如何解决
30	手写代码实现`kuai-shou-front-end=>KuaiShouFrontEnd`
