* ## 节点层次

  * DOM可以将任何HTML与XML文档描绘成一个由多节点构成的结构

  * 文档节点是每个文档的根节点，一般来说，文档节点只有一个子节点就是<html>元素，我们称之为文档元素。文档元素是文档最外层的元素，每个文档只能有一个文档元素。在HTML中，文档元素始终为<html>元素。

  * 每一段标记都可以通过树中的节点来表示。HTML元素通过元素节点表示，特性通过特性节点表示，文档类型通过文档类型节点表示，而注释通过注释节点表示。共计12种节点类型，这些类型继承自同一个基类型。

  * Node类型

    * DOM1级定义了一个Node接口，该接口将有DOM中的所有节点类型实现。Node接口是作为Node类型实现的。JavaScript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。

    * 每个节点都有一个nodeType属性，用于表明节点的类型。由以下的12个数值常量进行表示

      |      | 节点类型              | 描述                                                | 子节点                                                       |
      | :--- | :-------------------- | :-------------------------------------------------- | ------------------------------------------------------------ |
      | 1    | Element               | 代表元素                                            | Element, Text, Comment, ProcessingInstruction, CDATASection, EntityReference |
      | 2    | Attr                  | 代表属性                                            | Text, EntityReference                                        |
      | 3    | Text                  | 代表元素或属性中的文本内容。                        | None                                                         |
      | 4    | CDATASection          | 代表文档中的 CDATA 部分（不会由解析器解析的文本）。 | None                                                         |
      | 5    | EntityReference       | 代表实体引用。                                      | Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference |
      | 6    | Entity                | 代表实体。                                          | Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference |
      | 7    | ProcessingInstruction | 代表处理指令。                                      | None                                                         |
      | 8    | Comment               | 代表注释。                                          | None                                                         |
      | 9    | Document              | 代表整个文档（DOM 树的根节点）。                    | Element, ProcessingInstruction, Comment, DocumentType        |
      | 10   | DocumentType          | 向为文档定义的实体提供接口                          | None                                                         |
      | 11   | DocumentFragment      | 代表轻量级的 Document 对象，能够容纳文档的某个部分  | Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference |
      | 12   | Notation              | 代表 DTD 中声明的符号。                             | None                                                         |

    * nodeName与nodeValue

      用于了解节点的具体信息。对于元素节点，nodeName保存的始终是标签名，nodeValue的值始终为null

      ```js
      document.body.nodeName//"BODY"
      document.body.nodeValue//null
      ```

    * 节点关系

      * 每个节点都有一个childNodes属性，里面保存着一个NodeList对象，他是一个类数组对象，用于保存一组有序的节点，虽然可以通过【】来访问，但他不是Array的实例。NodeList他是实际上基于DOM机构动态执行查询的结果，因此会自动反映到NodeList对象中。

      * 如何访问NodeList中的节点，`[]`,`item()`

        举例：

        ```js
        document.body.childNodes[1]//div#wrapper
        document.body.childNodes.item(1)//div#wrapper
        ```

      *  每一个节点都有一个parentNode属性，该属性指向文档树中的父节点。包含在childNodes列表中的所有节点都有着相同的父节点，因此他们的oarentNode都指向同一个节点。此外，包含在childNodes列表中的每个节点相互之间都是同胞节点。通过使用列表中的每个节点的previousSibling和nextSibling属性，可以访问同一列表的其他节点。列表的第一个节点的previousSibling和列表的最后一个节点的nextSibling的值为null。父节点的firstChild与lastChild指向列表的第一个和最后一个节点。如果没有子节点都为null。hasChildNodes（）这个方法在节点包含一或多个子节点情况下返回true。

      * 所有的节点都有的最后一个属性是ownerDocument，该属性指向表示整个文档的文档节点。表示任何节点都属于他所在的文档，任何节点都不能同时存在于两个或更多文档中。

      * 注意：虽然所有节点的类型都继承自Node，但不是每种节点都有子节点

    * 操作节点

      * 方法
        * appendChild(). 用于向childNodes列表的末尾追加一个节点。添加节点后，节点信息会得到更新。更新完成后，appendChild（）会返回新增的节点。如果appendChild中的节点已经是文档中的一部分了，那结果就是将该节点从原来的位置转移到新位置。
        * insertBefore（）。如果需要把节点放在childNodes列表中的某个特点的位置上，而不是放在末尾，使用该方法。两个参数：要插入的节点和作为参照的节点。插入节点后，被插入的节点会变成参照节点的前一个同胞节点，同时会被方法返回。如果参考节点为null，会执行与appendChild一样的操作。
        * replaceChild（）。两个参数，要插入的节点和要替换的节点。替换的节点被返回，并且从文档树移除，同时及插入的节点占据其位置。在使用replaceChild（）插入一个节点时，该节点的所有关系指针都会从被他替换的节点复制过来。被替换的节点依然还在文档，但是它在文档中已经没有了自己的位置。
        * removeChild（）。接收一个参数，即将要移除的节点。返回被移除的节点。被移除的节点依然为文档所有，但是在文档中已经没有了自己的位置。
        * 注意：以上四个方法都是通过父节点的方法来操作子节点，只能在有子节点的节点进行使用。以下两个方法时所有类型的节点都有的。
        * cloneNode（）。用于创建调用这个方法的节点的一个完全相同的副本。cloneNode（）接受一个为boolean类型的参数，表示是否进行深复制。执行深复制复制节点及整个子节点树；浅复制，只复制节点本身，复制后的节点副本属于文档所有，但没有为他指定父节点。可以用以上四个方法将他添加到文档。注意：只复制特性，不会复制js属性
        * normalize（）。处理文档树中的文本节点。由于解析器实现或dom操作等原因，可能会出现文本节点不包括文本，或者接出现两个文本节点的情况。当某个节点调用这个方法时，就会在该节点的后代节点上查找这两种情况。找到空文本节点就删除，找到两个相邻的文本节点就合并为一个文本节点。

  * ## Document类型

    * 在浏览器中，document对象是HTMLDocument（继承自Document类型）的一个实例，表示着HTML页面。且document是window的一个属性，所以可以将其作为全局对象来访问。

    * 一些特性

      * nodeType为9
      * nodeName为“#document”
      * nodeValue为null
      * parentNode为null
      * ownerDocument为null
      * 子节点可能为DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction或Comment

      Document类型可以表示HTML页面或者其他基于XML的文档。

    * 除了上面的四种，还有的两个内置的访问子节点的快捷方式

      * documentElement：始终指向html元素
      * childNodes：访问文档元素
      
    * document.body: 取得对body的引用
    
    * document.doctype: 取得对<!DOCTYPE>的引用
    
    * 一般不会对document对象上调用对节点的增删操作，因为document是只读的
    
    * 文档信息
    
      * document.title:取得<title>里的内容，也可以进行修改
    
      * 网页请求
    
        * document.URL: 包含页面完整的URL
        * document.domain：包含页面的域名
        * document.referrer: 保存着链接到当前页面的那个页面的url ，没有页面来源的话，属性中可能包含空字符串
    
        信息都存于http头部，js只是提供方法访问
        这三个属性中，只有domain可以设置的。但由于安全方面的限制，并非随意可以给domain设置任何值。如果URL中含有一个子域名，例如p2p.wrox.com, 只能将domain设置为wrox.com. 不能将这个属性设置为url中不包括的域。
        用途：当页面中包含来自其他子域的框架或内嵌框架时，由于跨域安全限制，来自不同子域的页面无法通信。二通过将每个页面的域名设置为相同的值，这些页面就可以互相访问对方包含的js对象了。
    
        注意：松散的域名不能设置成紧绷的域名。
    
    * 查找元素
    
      * getElementById():  接收一个参数：要取得元素的id。如果找到相应的元素返回该元素，如果不存在带有相应元素的id，则返回null。严格匹配。如果出现多个，只返回文档中第一次出现的元素。
      * getElementsByTagName(): 接收一个参数，元素的标签名。返回的是包含零或多个元素的nodelist。在HTML'文档中，这个方法会返回一个HTMLCollection对象。访问方法：item()或者[].   HTMLCollection中还有一个方法namedItem()，(只会取得第一项)。HTMLCollection支持按名称访问，可以向方括号中传入数值或字符串形式的索引值。对数字索引对调用item(),对字符串索引会调用namedItem().  document.getElementsByTagName('*')会取得文档中的所有元素。html中不区分大小写，xml中区分大小写
      * getElementsByName(). 只有HTMLDocument中才有的方法，返回给定name特性的所有元素，常用是取得单选按钮
    
    * 特殊集合（都是HTMLCollection对象）
    
      * document.anchors, 包含文档中所有带name特性的a标签
      * document.forms, 与document.getElementsByTagName('form')得到的结果相同.
      * document.images, 与document.getElementsByTagName('img')得到的结果相同.
      * document.links,  包含所有带href特性的<a>元素
    
    * DOM一致性检测
    
      * DOM分为多个级别，包含多个部分。document.implementation属性提供了相应信息和功能的对象去检测浏览器实现DOM的哪些部分。DOM1级只提供了一个方法hasFeature()。接收两个参数：（要检测的DOM功能名称及版本号），支持返回true。
    
    * 文档写入
    
      * write()：接收一个字符串参数，即要写入到输出流的文本，原样写入，在页面加载过程中，动态写入内容
      * writeln()：接收一个字符串参数，即要写入到输出流的文本，会添加换行符'\n'，在页面加载过程中，动态写入内容
      * open()
      * close()
    
      write与writeln的问题：它可以动态包含外部资源，但是如果但是不能直接写`</script>  `因为他会把他当作第一个script的结束，后面的代码将无法执行，所以为了避免这个问题应该加入转义字符 `\`,即`<\/script>`. 在页面被呈现的过程中直接向其输入内容。但如果在加载结束后调用,那么输入的内容将会重写整个页面。
      open与close分别用于打开和关闭页面的输出流。如果是在页面加载期间使用write和writeln方法则不需要用到这两个方法。
    
  * ## Element类型

    * 提供了对元素标签名、子节点以及特性的发明和访问

    * 特征

      * nodeType：1
      * nodeName：元素的标签名
      * nodeValue：null
      * parentNode：Document或Element
      * 子节点：Element、Text、Comment、ProcessingInstruction、CDATASection或EntityReference

    * 进行nodeName或tagName比较的时候，应该转换一下大小写，返回的大小写形式不确定

      `element.tagName.toLowerCase() == "div"`

    * HTML元素

      * 由HTMLElement类型表示（继承自Element类型，添加了一些属性）
        * id：元素在文档中的唯一标识符
        * title：有关元素的附加说明信息
        * lang：元素内容的语言代码
        * dir：语言的方向
        * className：元素指定的css类

    * 取得特性`getAttribute()`

      * 可以针对任何特性使用
      * 给定名称的特性不存在，返回`null`
      * 可以取得自定义特性
      * 特性的名称不区分大小写
      * 自定义特性应该加`data-`
      * 任何元素的所有特性都可以用dom元素本身的属性来访问。不过只有非自定义的特性才会以属性的形式添加到dom对象中。所以自定义属性还是要用`getAttribute()`来访问
      * 有两类特殊的值，属性的值与`getAttribute`返回的值不同
        * style：通过`getAttribute()` 返回style特性值中包含的是css文档；通过属性来访问会返回一个对象
        * 事件处理程序：通过`getAttribute`访问，则会返回相应代码的字符串；访问onclick属性时会返回一个js函数
      * 因为存在上述差异，所以一般不适用getAttribute这个方法，而只是使用对象的属性，只有取得自定义属性的时候才会使用getAttribute

    * 设置特性`setAttribute`

      * 接收两个参数：要设置的特性名和值
      * 如果特性存在，会替换现有的值；如果不存在，会创建该属性并设置相应的值
      * 通过该方法既可以操作HTML特性也可以操作自定义对象。通过这个方法设置的特性名会被同一转成小写形式
      * 因为所有的特性都是属性，所以直接给属性赋值可以设置特性的值
      * 但是不能像`div.mycolor = 'red'`这样为dom元素添加一个自定义的属性，该属性不会成为元素的特性

    * 删除元素的特性`removeAttribute()`

      * 不仅会清除特性的值而且用于彻底删除元素的特性
      * 方法不常用
      
    * 举例：`div.removeAttribute('class');`

    * attributes属性

      * Element类型时使用attributes属性的唯一一个DOM节点类型。attributes属性中包含一个NamedNodeMap，与NodeList类似，也是一个动态的集合。元素中的每一个特性都由一个Attr节点表示，每个节点都保存在NamedNodeMap对象中。
      * NamedNodeMap方法
        * getNamedItem(name)：返回nodeName属性等于name的节点
        * removeNamedItem(name)：从列表中移除nodeName属性等于name的节点
        * setNamedItem(node)：向列表中添加节点，以节点的nodeName属性为索引
        * item(pos)：返回位于数字pos位置处的节点
      * attributes属性包含一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeValue就是特性的值，取得特性的值`var id = element.attrtibutes.getNamedItem('id').nodeValue;` 简写： `var id = element.attrtibutes["id"].nodeValue;`
      * 调用removeNamedItem()方法与在元素上调用removeAttribute()方法的效果相同-直接删除具有给定名称的特性。区别是removeNamedItem()返回表示被删除特性的Attr节点
      * 方法不够方便，一般会使用getAttribute(),setAttribute(),removeAttrubute()
      * 不够如果要遍历元素的特性使用attributes会比较方便
      
    * 创建元素

      * document.createElement()可以创建新元素。接收一个参数，即要创建元素的标签名。不区分大小写
      * 在创建的同时也为新元素设置了ownerDocument属性、
      * 创建后需要把元素添加到文档树中才会影响浏览器的显示

  * ## Text类型

    * 特征
      * nodeType：3
      * nodeName：#text
      * nodeValue：节点所包含的文本(可以用nodeValue或data属性来访问Text节点中的文本，进行修改的时候小于号大于号都会被转义)
      * parentNode：是一个Element
      * 子节点：不支持（没有）子节点
      * appendData(text)：将text添加到节点的末尾
      * deleteData(offset, count)：从offset指定的位置开始删除count个字符
      * insertData(offset, text)：从offset指定的位置插入text
      * replaceData(offset, count, text)：同text替换从offset指定的位置开始到offset + count为止处的文本
      * splitText(offset)：从offset指定的位置将当前文本节点分成两个文本节点
      * substringData(offset, count)：提取从offset指定的位置开始到offset + count为止的字符串
      * length：保存着节点中字符的数目
      
    * 在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确认有内容存在

    * 创建文本节点
      
      * document.createTextNode()。接收一个参数-要插入节点中的文本。作为参数的文本将按照HTML与XML的格式进行编码。
      * 创建新文本节点的同时，也会为其设置ownerDocument属性。不过，除非把新节点加入到文档树已经存在的节点中，否则我们不会在浏览器窗口看到新节点
      * 一般情况，每个元素只能有一个文本节点（当自己往元素中添加两个文本节点就会出现两个文本节点）
      * 如果两个文本节点是相邻的同胞节点，那么这两个节点就会连起来显示，中间不会有空格
      
    * 规范化文本节点

      normalize()：如果在一个包含两个或多个文本节点的父元素上调用normalize()方法，则会将所有文本节点合并成一个节点，结果节点的nodeValue等于将合并前每个文本节点的nodeValue值拼接起来的值

      浏览器在解析文档时永远不会创建相邻的文本节点。这种情况只作为执行DOM操作的结果出现

    * 分割文本节点

      splitText()：会将一个文本节点分成两个文本节点。按照指定的位置切割nodeValue值。原来的文本节点包含从开始到指定位置之前的内容，新文本节点包含剩下的文本。有着和之前相同的父节点。

  * ## Comment类型

    * 注释使用Comment类型来表示的
    * 特征
      * nodeType：8
      * nodeName：“#comment”
      * nodeValue：是注释中的内容
      * parentNode：Document或Element
      * 不支持子节点
    * Comment类型与Text类型基础自相同的基类，因此他具有除了splitText()以外的所有字符串操作方法。与Text类型类似，也可以通过nodeValue与data属性来取得注释的内容
    * 创建注释节点：document.createComment("a comment");

  * ## CDATASection

    * CDATASection类型只针对XML文档，表示的是CDATA区域。与COmment类似，继承自Text类型。拥有除splitText ()以外的所有字符串操作方法
    * 特征
      * nodeType：4
      * nodeName：“#cdata-section”
      * nodeValue：CDATA区域中的内容
      * parentNode：Document或Element
      * 不支持（没有）子节点
    * 因为CDATASection只在XML中出现，所以浏览器会把CDATASection错误的解析成Comment或Element
    * 在真正的XML文档中，可以使用document.createCDataSection()来创建CDATA区域

  * ## DocumentType

    * 在web浏览器不常用，包含着doctype有关的所有信息
    * 特征
      * nodetype：10
      * nodeName：doctype的名称
      * nodeValue：null
      * parentNode：Document
      * 没有（不支持）子节点

  * ## DocumentFragment（文档片段）

    * 所有的节点类型中。只有DocuemntFragment在文档中没有对应的标记。它是一种轻量级的文档，可以包含和控制节点，但不会像完整的文档一样占用额外的资源。
    * 特征
      * nodeType：11
      * nodeName：“#document-fragment”
      * nodeValue：null
      * parentNode：null
      * 子节点可以是Element，ProcessingInstruction，Comment，Text，CDATASection或EntityReference
    * 虽然文档片段不能直接提娜佳到文档中，但是可以当作仓库使用，保存之后可能添加到文档树中的节点。
    * 创建 document.createDocumentFragment()
    * 继承了node的所有方法

  * ## Attr类型

    * 元素的特性DOM以Attr类型来表示。特性就是存在于元素的attributes属性中的节点

    * 特征

      * nodeType：2
      * nodeName：特性的名称
      * nodeValue：特性的值
      * parentNode：null
      * 在HTML中不支持（没有）子节点

    * 尽管特性是节点，但是却不被认为是DOM文档树的一部分，一般是用方法来获取值么人不是引用节点

    * Attr对象有三个属性

      * name：特性名称
      * value：特性的值
      * specified：是一个bool值，用于区别特性是在代码中指定的还是默认的

    * 创建特性节点

      可以用document.createAttribute()创建一个新的特性节点，然后添加完特性后，可以用element.setAttributeNode(attr)将新创建的特性添加到元素中。getAttributeNode方法与attributes属性返回的是特性的Attr节点，而getAttribute只返回特性的值

    * 不建议访问特性节点，使用getAttribute，setAttribute，removeAttribute方法比操作特性节点更为方便

* # DOM操作结束

  * 动态脚本

    * 是指页面加载时不存在，但将来的某个时刻通过修改DOM动态添加的脚本

    * 创建动态脚本

      * 插入外部文件

        * 举例

          ```js
          var script = document.createElement("script");
          script.type = "text/javascript";
          script.src = "client.js";
          document.body.appendChild(script);
          ```

          只有添加到页面才会加载外部文件

      * 直接插入js代码

        * 举例

          ```js
          var script = document.createElement("script");
          script.type = "text/javascript";
          script.appendChild(
          	document.createTextNode("function sayHi() {console.log(123)}")
          );
          document.body.appendChild(script);
          ```

        * 如果因为IE把script当成一个特殊的节点不允许访问子节点，不能使用appendChild，只能用`script.text = ‘’ `去添加代码

  * 动态样式

    * 页面加载后动态添加到页面里的

    * 两种创建方式

      * link

        * 举例

          ```js
          var link = document.createElement("link");
          link.rel = "stylesheet";
          link.type = "text/css";
          link.href = "style.css";
          var head = document.getElementsByTagName("head")[0];
          head.appendChild(link);
          ```

          必须把Link传到head里面而不是body，才保证所有浏览器中的行为一致

      * style

        * 举例

          ```js
          var style = document.createElement("style");
          style.type = "text/css"
          style.appendChild(
          	document.createTextNode("body{margin: 0;}")
          );
          head.appendChild(style);
          ```

        IE把style当成一个特殊的节点不允许访问子节点，所以添加css文本应该是`style.styleSheet.csstext = 'body{margin: 0;}'`

