与传统的病毒相比，宏病毒的最新特色是它们不依赖操作系统。

宏病毒利用宏语言外部的例程的调用能力如使用Windows API函数，能进行任何操作。与传统的病毒相比，宏病毒的最新特色是它们不依赖操作系统。只要有应用程序支持解释，宏病毒无需改动可在许多平台上运行。例如Microsoft Word宏病毒能在任何安装过Microsoft Word的系统中运行（Windows3.1x,Windows 95,Windows NT,MAC等）。


打开 VB 编辑器 

方法一：从功能区打开。点击功能区—开发工具—Visual Basic 命令打开

方法二：快捷键打开。按「Alt + F11」打开。

方法三：从工作表查看代码命令打开。鼠标移动到任意一工作表名称处(如 sheet1),点击右键，在菜单中选择「查看代码」打开。

<br/>

点击 视图下拉菜单下的 立即窗口，打开 立即窗口

<br/>

office VB 视图界面重置：  点击 视图下拉菜单下的工程资源管理器  ，右键单击 VBAProject, 勾选 "可连接的"




在VBAProject 下新建 模块1。

写入代码
	
	Sub MyCode()
        
    	Sheet1.Range("A1") = "Hello World"

	End Sub

点击绿色的运行图标。在excel中查看 Sheet1工作表 A1 单元格已经写入了我们指定的内容。


	Sub DimTest()
    
	    Dim i As Integer '声明整数类型变量
	    
	    Dim name As String '声明文本类型变量
	    
	    Dim rng As Range '声明单元格类型对象
	    
	    Dim book As Workbook '声明工作簿对象
	    
	    name = "VBA 工作簿" '设置 name 变量的值
	    
	   
	    'book.name = name '设置工作簿名称
    
	End Sub


****
	
	
	Sub IfTest()
	
	    Dim i As Integer
	    i = 10
	    If i > 0 Then
	         MsgBox "我被执行了"
	    End If
	       
	End Sub
	
	
	Sub IfTest2()
	Dim i As Integer
	Dim sh As Worksheet
	    For i = 1 To 31
	        Set sh = Worksheets.Add  '新建一个工作表
	        sh.name = i
	    Next
	End Sub






**在视图，点击对象浏览器 可以查看VBA所有对象及属性**

#### Dim

Dim 是 Dimension(尺寸) 的缩写，用来声明代码中的变量。意思是我们告诉 VBA ，某一变量是文本还是数字还是单元格区域。








...

...

...


<br/>








+  windows  预览功能自动触发 宏