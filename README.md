# forge-problem

###  Forge Viewer加载模型在浏览器中不显示问题解决及扩展

##### 作者：丁香尚人  日期：2017-12-25  邮箱：quanlanguage@gmail.com 鸣谢：Lxh，Ayers 这二位微信好友
---
  首先要说下为什么写这篇文章，是由于昨晚一位朋友在使用forge Viewer加载模型的时候
遇到浏览器不兼容的问题，所以我首先会分析为什么出现这个问题，如何解决这个问题，然后在扩展我在使用过程中遇到问题以及相应的工具使用

1.forge技术的基础概述

2.问题描述

3.如何解决这个问题以及每种方法优缺点

4.谈到forge不得不谈HTML5

5. 浏览器的区别

6.forge技术必将是时代的潮流

7.学习forge技术个人心得

7.1如何去理解canvas

7.2 javascript服务器端的实现

7.3 在用使用nodejs过程中常用命令

7.4 forge技术的个人的推到过程（可能有错误望指正）

7.5 我怎么调试forge项目

#### 1.forge技术的基础概述
  很多初学者在刚接触forge时，都不明白forge的技术基石---canvas。如果浏览器对canvas支持不是很完善，那么你就很容易出现，在A浏览器能成功打开模型，在B浏览器打不开模型，这种情况在手机端体现特别明显。在这里你需要二点，forge技术的基石是canvas与如果浏览器对canvas支持不是很好友好那么容易出现不兼容现象。在后面的我会详细的讲解。
#### 2.问题描述
  我的那位朋友遇到一个这样的问题，在外网部署了一台服务器方便用户浏览模型，用户在微信当中打开模型能正常加载，但是在qq浏览器当中不能正常加载，在谷歌浏览器当中能正常加载。
#### 3.如何解决这个问题以及每种方法优缺点
  首先我们要明确这种问题是在浏览器加载模型出现问题，也就是canvas“画”模型失败。若是出现这个问题很多时候会发现一一个这样的现象，页面什么都没有。这里我把这种表象归类为javascript无法正常进行函数回调并抛出异常（稍后我会详细解释什么是javascript函数回调）。本质原因我会在后面详细的讲解原因，现在我对这种表象原因提出二个解决方案。
  1. 在用户访问模型的时候，首先将用户浏览器信息发送个服务器并返回结果判断是否加载模型。
  ```
      C#代码
      Request.Browser.MajorVersion.ToString();//获取客户端浏览器的（主）版本号
      Request.Browser.Version.ToString();//获取客户端浏览器的完整版本号
      Request.Browser.Platform.ToString();//获取客户端使用平台的名字
      Request.UserHostAddress.ToString();     //获取远程客户端主机IP
  ```
  2. 在浏览器端用javascript写一个try--catch语句来解决，如果用户能正常加载模型，则不会抛出异常，反之则反。
  ```
    try{
        加载模型语句
    }
    catch(err){
       alert("你的浏览器无法正常加载模型，请更换其他浏览器")
    }
  ```

  这二种方式是分别从服务器端与客户端解决这种不兼容问题，各自有各自的有点。接下来分析二种解决方式的优缺点（有不同意见的联系作者）。

  第一种方式解决：优点：精确判断用户是否能加载模型，有利于对模型进行优化等。缺点：消耗服务器性能，实现繁琐。
  第二种方式解决：优点：实现简单，不消耗服务器性能。缺点：不利于对模型的优化。

  ---
  #### 以下内容为方便大家更加深刻的了解forge（以下大部分为个人经验，个人见解居多）
  ---
  #### 4.谈到forge不得不谈HTML5
  很多人了解web技术的时候，都忽略了一个人的推动--伯纳斯·李（互联网之父）。整个互联网的发展与这个人息息相关，只是我们很多人不知道而已。

  第一家互联网上市公司网景公司（已经倒闭），世界上第一款商业浏览器开发商，与现在的火狐浏览器有很大关系。网景公司在上市之前，专门拜访伯纳斯·李说以后网景公司是否要给伯纳斯·李专利费，伯纳斯·李放弃了将web技术申请专利，并说到互联网技术属于全人类。这就开启了各大公司对web技术的理解不一致的开端。

  在微软将ie作为windows默认浏览器，促使网景公司浏览器市场占有率大幅度下降，IE浏览器与网景浏览器在对HTML，CSS，javascript理解存在比较大的差异。就在此时伯纳斯·李看到这种情况，认为web的发展走向封闭，就在此时他利用自己的威望，促使大部分厂商加入万维网联盟，制定准则促使各大浏览器遵守其准则。但是各大厂商在理解准则上存在部分差异，但是促进了互联网技术的飞速发展。于此同时出现了一个非常著名的网站，禅意花园，尝试将所有的浏览器的差异消除，影响了后面无数的网站，也是早期探索web技术该何去何从的先驱者。

  万维网联盟制定了很多标准，有些标准过于苛刻遭到各大公司排斥，但是最为人熟知的是html4.01。现在还是很网站还是使用这个标准，一般使用这一套标准的网站，那么网站一般会支持flash。这我们就要讲到Adobe公司，很多人都知道Adobe公司的photoshop，但是你可能不知道这家公司在web技术飞速发展过程中也是获利比较多的公司，为什么会这么讲，因为只要你使用flash技术，那么你需要向adobe公司交flash授权费。当乔布斯回到苹果公司的时候，看到这笔巨大的开支并且w3c联盟多年没有颁布新的准则，联合各大厂商包括google，微软，autodesk等大公司参与Html5准则的制定，adobe在认识到这是时代的潮流，才被迫加入。

  很遗憾的是在制定html5规范过程中，没有中国互联网公司的加入。苹果公司是html5技术的先驱者，很多技术都是为自己服务的。其中最为明显的二个就是canvas与不依赖flash直接播放视频。谷歌浏览器与safari有一段往事，这里我们就不提了。html5早期在苹果产品上体验非常好，如果你是windows的用户或者linux用户可能不是很好，这也是为什么我们叫html5很响，而雨声很小的原因。

  #### 5. 浏览器的区别
  前面我们讲了最重要的二点，一各大浏览器对准则理解存在差异，苹果公司是早期html5倡导者。

  至于这二点我们会做深层次的分析。各大厂商为什么会对同一套准则理解不一样，这里我们提及的厂商为参与了规则的制定的厂商，不是那些披着国产，或自己研发的浏览器的厂商。

  1.规则制定时，大概细节未做明确规定。

  2.大家为了保护自己市场占有率为前提。

  至于其他作为笔者不想明确指出，我认为这二点是非常重要的。说了这么多我们就来讲一讲，浏览器的主要内核，ie内核，Webkit内核（谷歌浏览器），Gecko内核（火狐浏览器），Presto内核（Opera浏览器），Safari内核（苹果浏览器）。前面我们讲到苹果是html5先驱者，所以safari内核对html5支持比较友好，但是由于是先驱者很多其他那些内核组织被迫接受苹果的部分设计理念，但是safari内核是不开源的，这也促使谷歌公司以safari内核为原型设计了webkit内核。由于其他的厂商由于不具备开发浏览器实力或开发成本过高，选择参加社区版开源项目。而这里我们的autodesk公司就与google公司抱团了，所以建议在google浏览器调试。

  这里我讲了很多国外公司或只讲浏览器内核，为什么不直接讲一讲我们国产的浏览器，因为国产浏览器厂商由于没有参与了规则制定或由于研发成本过高等原因。我也希望我们的国产浏览器能发战壮大，当时由于没有先天优势，纷纷如同一首歌唱的披着羊皮的狼，为什么我会把国产浏览器说成披着羊皮的狼，羊是因为我们技术薄弱，狼是为了打造生态全，为自家的网站服务，开发者工具都非常难找，而对开发者不友好。同时有一个最大的弊病就是，国产浏览器由于是用其他浏览器内核，往往更新与支持html5方面会比较迟钝，或者出现隔代更新的问题。这也是我们刚开始的那个问题产生的一个原因。

  我们讲了浏览器内核，由于autodesk在设计forge的时候提出留了一个理念是就是全平台都能随时浏览。我需要详细的介绍我们的手机端。手机的操作系统主要分为ios和android。至于forge对苹果设备是支持相当友好的，你一点都不用当心。但是android内置浏览器与google浏览器是很类似的，但是存在差异。你也大可不必当心，因为autodesk应该早就注意到了并解决了。你需要注意我们的国有厂商的浏览器（应用内部浏览器大部分为android浏览器，我们不能一棒子打死，因为这个很难判断），qq浏览器，360浏览器，uc浏览器，这些浏览器是国内厂商的浏览器，大部分存在更新比较滞后的问题。


  #### 6.forge技术必将是时代的潮流。

  我们前面提到国内各大浏览器对forge支持不是很友好，会不会严重制约这项技术的发展，答案是不会不制约，会慢慢的发生改变。你可以看一看5年前的html5技术与现在的html5技术。发展迅速，最主要的驱动是虚拟技术，人工智能的重大突破，苹果的产品的成功等待原因，促使那些国有浏览器厂商不断的完善自身，支持html5技术。同时现在forge在国内推广比较慢，原因在于庞大的用于使用的电脑的浏览器很大程度不支持html5技术，第二国内的接受这种订阅的概念需要时间。你会问我这个forge有点贵，我也是这么觉得，但是十年前花钱去看电影，很多人不太愿意，现在花钱看电影大家感觉很正常的一种现象。同时我建议你去读比尔盖茨早年的一封信：给玩家的一封信。这封信是促使软件收费的直接原因也是互联网发展过程中最重要的转折点。


  #### 7.学习forge技术个人心得

  ##### 7.1如何去理解canvas
  很多人刚接触canvas是使用html5标签，但是你可能不知道canvas标签最早由Apple引入WebKit，用于Mac OS X 的 Dashboard，后来又在Safari和Google Chrome被实现。前面我们在将html5的历史时提到苹果是html5技术的倡导者，初衷就是去Adobe技术（个人理解）。你可以发现canvas很多时候与photoshop很相似，这里的很相似是底层的设计模式。

  canvas核心的概念就是画布，什么是画布？我们再讲这个问题时，不需要复杂化，只需要知道那个是一个与黑板很相似的东西。我们要在黑板上画一条直线，那么我们就要拿一个粉笔。我们可以类推出如果我们要在canvas上画一条直线，那么就需要创建一个类似一个粉笔的东西---画笔。有一个canvas你可以画出与各种各样的图案，同时canvas这项技术应用不局限在画那些简单的图案上，我们的forge就是建立在canvas技术上，在你惊叹forge技术的先进性时候，你可能没有注意到apple与google正在将这项技术运用在深度学习领域（这里你可以去google上搜索）。

  canvas技术说起来相当简单，二个概念画笔与画布。你如何去使用他那就要靠你的想象力与数学功底。在这里我给你介绍一个系列的书来帮助你对于工程领域的数学的运用《俄罗斯数学教材选译》。这个系列的书籍为我国培养的许多优秀的数学工作者。

  ##### 7.2 javascript服务器端的实现

  在计算机语言界，不乏出现天才。这里我们讲到的javascript的设计者布兰登·艾奇，这位年仅34岁的开发者花了十天为网景公司开发了javascript这种浏览器端的脚本语言。布兰登·艾奇这个人如果你感兴趣的话可以去google上搜寻下，知道他的人很少，也是与他的个性相关，这里我就不扩展的讲有关他的事情。这里我们提到了一家公司网景公司，javascript是运行在浏览器端的脚本语言（非服务器端脚本语言）。在很长的时间里大家都认为javascript作为服务器端的开发语言是性能比较差，或是不可能实现。

  javascript如果作为服务器端的开发语言那么必须有一点要做到，那就是能读取服务器磁盘的的文件，实现了这个过程。javascript才算有成为服务器端开发语言的资本，由于javascript长时间作为客户端的脚本语言，很少人有做过相应的类库。直到2009年2月，Ryan Dahl在博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库。这里引入了一个概念V8引擎，那什么是v8引擎？

  如果问我v8引擎到底是什么我也说不清楚，因为没有做太深的了解。我可以做一个类比。C#语言的虚拟机是.NET Framework,java也有自己的虚拟机，我们可以把V8引擎看作是javascript虚拟机来运行javascript代码，同时也扩展了对c++的支持。最重要的一点v8引擎也是作为google 浏览器的一部分，自己领会下。

  讲了v8引擎，是时候讲下我们的nodejs，这是我们在forge中经常遇到的一个。noedejs是服务器端的javascript框架，当我们在使用第三方的Package的时候我们就需要用到npm下载Package。

  NPM的全称是Node Package Manager，是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。

  如果你熟悉ruby的gem，Python的pypi、setuptools，PHP的pear，那么你就知道NPM的作用是什么了。

  Nodejs自身提供了基本的模块，但是开发实际应用过程中仅仅依靠这些基本模块则还需要较多的工作。幸运的是，Nodejs库和框架为我们提供了帮助，让我们减少工作量。但是成百上千的库或者框架管理起来又很麻烦，有了NPM，可以很快的找到特定服务要使用的包，进行下载、安装以及管理已经安装的包。

  有的时候我们也会用到cnpm全称是：China Node Package Manager。作为初学者遇到最多的问题，下载不到Package或是不只npm与cnpm的差别。你从二者全称就能得到答案

  ##### 7.3 在用使用nodejs过程中常用命令

    npm init 会引导你创建一个package.json文件，包括名称、版本、作者这些信息等（我会理解称项目的初始化）
    npm install package-name  安装package
  我个人认为作为初学者知道这二个命令是最基础的要求，那么当你使用npm命令的时候你需要知道你下载package的源极有可能来自国外，而你使用cnpm的时候使用的源来自国内。npm的源里面的package是包含国内的源甚至还要大于，有些时候国内的开发者不把自己开发的package上传到国外的源上去。

  源到底是什么东西？我也不好解释什么是源，如果你玩过linux操作系统的时候，例如ubuntu，我们在安装软件的过程中会使用apt-get install package，其中npm与apt-get有着类似的作用。同时你也会遇到为什么有的时候下载不了package，你百度搜索。很多博客会告诉你，改成阿里云的源。ubuntu的这种现象与nodejs npm安装不了package非常的类似。那么我可以做一个大胆的假设源就是一个存储大量package的ftp服务器，怎么论证他是一个类似的ftp服务器，一个很简单的论证方法，你将源的地址复制到地址栏访问，你会看到很多类似的目录。

  上面大致解释了一下什么是源这个概念，我们如何去解决什么我们下载不了package的问题了，如果你使用npm命令安装package的时候，我会建议你开代理，如果你使用cnpm安装package极有可能下载不到最新的package与找不到制定的package。至于如何抉择在于你抱有何种态度于看法。

  cnpm跟npm用法完全一致，只是在执行命令时将npm改为cnpm。至于详细的介绍等待你的探索。

##### 7.4 forge技术的个人的推到过程（可能有错误望指正）

  在此之前我给你介绍了很多关于forge背后的故事，你大概有会困惑其中那么多技术在forge中用到了嘛。我的答案是当然用到了，在你的平常不经意当中。

  canvas作为html5中的一个重要的特性，nodejs作为javascript端运行的框架（autodesk官方很多的dome就是使用这个），nodejs是基于谷歌浏览器的V8引擎外加c++/c的类库的一个服务器端的框架。与此同时你会有疑问，我可以不使用nodejs作为服务器框架，使用java，C#等技术来做，你这种猜想是对的。你完全可以不使用nodejs作为web端的首选框架，但是你一定要引用autodesk所提供的官方的javascript类库，若你使用了那些类库，你就会接触一个很常见的概念，函数回调。

  在这里我来大概给你解释下什么是函数回调。在javaweb当中我们会遇到一张编程技巧，链式编程的思想。什么是链式编程思想，很简单的说就是是将多个操作（多行代码）通过点号(.)链接在一起成为一句代码,使代码可读性好，如：a(1).b(2).c(3)。这么做有什么好处了，就是你用完这个对象就会销毁。发扬光大，利用最为广泛的是用在javascript编程当中，我们在Autodesk官方的资料看到，如果你调用这个方法的时候会回到这个，为什么官方会采取这种链式的编程思想，我个人感觉最重要的一点就是，由于javascript运行在用户的浏览器当中，通过浏览器的javascript解析器（如v8解析引擎），解析器有相应的垃圾回收机制。有垃圾回收引擎，为什么还要链式编程？如果我们不采取链式编程，若垃圾得不到有效回收，那么浏览器就会出现无法响应的现象。你要想验证这个很简单，你打开100个网页在浏览器当中，出现浏览器无法响应的概率很大。还有另外一方面就是浏览器在运行这个网页时分配的内存是有限的，你可以在一个空白的页面上写一个死循环验证一下这个。

  我们讲了一下调用autodesk官方的javascript类库为什么会看到回调。同时你需要注意我们什么时候回有回调，举一个简单的例子，我们操作canvas里面的模型那么就一定回回调重新加载的方法，为什么这么做？原因很简单，我们在讲canvas是讲到画布，如果你想把黑板上一个字擦调，那么你需要用到黑板擦，现在我们类比到canvas里面就是重新初始化不同参数达到同样的效果。

  那forge那些部分不会用到canvas了，我做了一个大概的总结，那就是在网页上显示鼠标箭头（默认样式）和小手（我的maac上显示的）大部分都会不是用的canvas画的，当我们把鼠标放在canvas区域的时候，autodesk的官方js做了一个操作，那就是将默认的鼠标样式更改成转化视角的样式（我自己这么理解的）并添加相应的方法。这紧紧是我个人的经验而已，我大部分都是都是将forge中的按钮归类与不用canvas实现的而是用原生的html标签添加相应的标签与定位完成的。

  那么canvas如何画出相应的模型了，至于具体怎么实现将模型画在画布上，就是通过画笔做到的，我认为有几步

  1.将数据模型上传到forge 官网提供的转化网址，将cad文件转化成具备相应格式的文件，例如json格式，等类型

  2.autodesk官方提供相应的javascript的类库解析相应的从forge 官网得到的转化之后的数据，并初始化canvas模型

我个人认为这二部不是forge实现的核心逻辑与步骤，我在做forge项目的过程中经常会遇到不联网加载不了模型，有些js不加载无法正常加载模型。我猜测forge实现需连接特定服务器或加载指定的js文件。

##### 7.5 我怎么调试forge项目

  相信你看完了，对forge技术有一个大概的了解，那么我们怎么把这些知识运用在我们编程当中了。

  现阶段对于开发者面临最大的问题我认为有一下三点：

  1.浏览器不兼容性，（对浏览器内核，javascript引擎不是很了解）

  2.使用官方的dome是无法正常运行，（package不能正常加载，网址无法访问，那些js无法加载）

  3.不清楚那些那些是canvas实现的，那些不是canvas实现的，造成自己不知如何实现制定功能

  至于以上三点，我个人的经验是用火狐浏览器进行调试，观看相应的js，css，加载情况。查看相应的错误提示来帮助自己迅速判断是属于那一类问题。调试完了之后建议使用谷歌浏览器在调试下，最后在苹果设备上调试。
