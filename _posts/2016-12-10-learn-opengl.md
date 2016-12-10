---
layout: post
title: 期末到了--最后两周OpenGL从入门到放弃
comments: true
categories: 
- OpenGL
- CPP
---  
### 学习资料：[learnOpenGL](www.learnOpenGL.com) 对应的有 [中文版](http://bullteacher.com/)  
还剩两周就要考OpenGL了，上课没好好听讲，心塞。  

从我个人的理解来看，OpenGL称为一个状态机，表现了对原始数据经过若干道的工序，分别对其进行加工，最终输出。  

一个最基本的例子，画一个三角形：  
`顶点 -->  OpenGL内部可以处理的顶点(坐标在[-1，1]区间) --> 指定每个顶点的颜色 --> 绘制图像`  

其中将顶点转换成OpenGL可以使用的顶点，是顶点着色器(Vertex Shader)的工作，为每个顶点指定颜色，是片段着色器的工作(Fragment Shader)。这里说的不是那么准确，实际上对于原始数据的处理，经过了这些着色器：

![](https://learnopengl.com/img/getting-started/pipeline.png)  

那么这两个着色器要怎么使用呢？着色器代码是一门特定的语言：`GLSL`。   
这是一种类C的语言，先来看一下简单的顶点着色器代码:  

    #version 330 core 
    layout (location = 0) in vec3 position; 
    void main()
    {
        gl_Position = vec4(position.x, position.y, position.z, 1.0);
    }  

`version 330`表示OpenGL版本3.3，`core`表示启用core profile模式。`layout (location = 0)`给这个定义的变量`position`一个索引，之后会用到。`in`表示输入的变量,`out`表示输出的变量,`vec3`为数据类型(关键字),表示一个三维向量。`gl_Position = vec4(position.x, position.y, position.z, 1.0);`表示定义一个`gl_Position`，它是一个`vec4`的四维向量，四维分别为`position.x`, `position.y`, `position.z, 1.0`。  
这样，我们就把一个从外部输入的三维向量，转换成OpenGL可以处理的向量，虽然这里我们没有对输入向量做什么改变。  
假设我们有一个数组表示三维空间中的三个点：  

    GLfloat vertices[] = {
        -0.5f, -0.5f, 0.0f,//第一个点
         0.5f, -0.5f, 0.0f,//第二个点
         0.0f,  0.5f, 0.0f //第三个点
    };  

我们就可以这个顶点数组传入顶点着色器，如果坐标不是标准坐标(大小位于[-1,1])，那么就需要在顶点着色器中进行改变。  
可惜顶点着色器不能简单从一个顶点数组获取值，我们需要将这个顶点数组存入内存中，为了效率我们将这个数组存入显存，使用一个称为`顶点缓冲对象VBO`的对象来管理这个内存，在创建它之前，有一些东西需要说明：  
1.在OpenGL中，对象的表现形式为一串无符号整形数据，把它当作你所创建对象的句柄好了。  
2.OpenGL为状态机，所以表现为一个个状态的设定。  

创建对象，声明对象的方法为:  

    //创建对象 不允许使用自定义名字  
    GLuint objectName;  
    glGenObject(1, &objectName);  

    // 设置对象状态
    glBindObject(GL_MODIFY, objectName);  
    glObjectParameteri(GL_MODIFY, GL_OBJECT_COUNT, 5);    

现在我们来创建一个VBO：  

    GLuint VBO; 
    glGenBuffers(1, &VBO); 
    glBindBuffer(GL_ARRAY_BUFFER, VBO);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);  

VBO全称为：Vertex Buffer Object,
想创建一个Buffer，自然使用GenBuffers方法，第一个参数表示创建VBO的个数，比如我们可以使用：  

    GLuint VBO[2];
    glGenBuffers(2, &VBO); 

来创建两个VBO，之后将VBO绑定在一块显存空间中，数组类型所存放的空间为`GL_ARRAY_BUFFER`，绑定后，使用`glBufferData`将数组存放在这块显存，第一个参数是我们希望把数据复制到哪儿，第二个参数指定我们希望传递给缓冲的数据的大小（字节）；用一个简单的sizeof计算出顶点数据就行。第三个参数是我们希望发送的真实数据。
第四个参数指定了我们希望显卡如何管理给定的数据。有三种形式：  
`GL_STATIC_DRAW`：数据不会或几乎不会改变。  
`GL_DYNAMIC_DRAW`：数据会被改变很多。  
`GL_STREAM_DRAW`：数据每次绘制时都会改变。  

顶点缓冲对象创建完毕，接下来创建一个顶点着色器：  

    GLuint vertexShader;
    vertexShader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertexShader, 1, &vertexShaderSource, null);
    glCompileShader(vertexShader);  

`glCreateShader`创建一个着色器，返回其句柄，参数为着色器种类。`glShaderSource`加载着色器代码到着色器对象上，第一个参数为着色器对象，第二参数指定了源码中有多少个字符串，第三个参数是顶点着色器真正的源码，第四个参数先设置为`NULL`。`glCompileShader`编译着色器。

偏离了方向，卒。  
看教程去了。