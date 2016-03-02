title: 【OCR/机器学习】基于 Tesseract的字体搜索引擎
date: 2016-3-1 19:37:09

tags:

- 机器学习
- 神经网络
- 图像识别
- 搜索引擎

categories:

- 原创博文
- 自己的开源项目

---
# 前言：

这是一篇图像识别OCR技术、机器学习、以及简易搜索引擎构建相关的技术Blog，是自己在做毕设的同时，每天不断记录研究成果和心得的地方。

<!--more-->

# 选题背景

OCR（Optical Character Recognition 光学字符识别）技术，是指电子设备（例如扫描仪或数码相机）检查纸上打印的字符，通过检测暗、亮的模式确定其形状，然后用字符识别方法将形状翻译成计算机文字的过程。 

Tesseract的OCR引擎最先由HP实验室于1985年开始研发，至1995年时已经成为OCR业内最准确的三款识别引擎之一。然而，HP不久便决定放弃OCR业务，Tesseract也从此尘封。 
数年以后，HP意识到，与其将Tesseract束之高阁，不如贡献给开源软件业，让其重焕新生－－2005年，Tesseract由美国内华达州信息技术研究所获得，并求诸于Google对Tesseract进行改进、消除Bug、优化工作。 Tesseract目前已作为开源项目发布在Google Project，其最新版本3.0已经支持中文OCR。

在这样成熟的技术背景下，我很想利用这项OCR技术，再结合当下热门的移动互联网的开发技术和信息检索技术，实现一个能将陌生字体成功识别的移动端搜索引擎，帮助更多的设计工作者，旨在为他们工作时候能更加快捷、准确地获取字体信息。同时也希望字体创作者，能有一个很好的平台展示、上架自己的字体作品，帮助他们更好地推广行业风气和创作文化。

# 需求分析




# 用例设计
![](http://7xi6qz.com1.z0.glb.clouddn.com/%E6%AF%95%E8%AE%BE%E5%AD%97%E4%BD%93%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E-%E7%94%A8%E4%BE%8B%E5%9B%BE%E5%88%9D%E7%A8%BF.png)

# 应用领域
- 纹身设计
- 海报设计
- 广告设计
- 网站版本设计
- AppUI设计
- 考古发现



# 架构设计


![](http://7xi6qz.com1.z0.glb.clouddn.com/%E6%AF%95%E8%AE%BE%E5%AD%97%E4%BD%93%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E.png)


# 技术点分析




# 工程实现



# 测试数据


# 心得体会

# 参考文献

- [Github—Tesseract-OCR](https://github.com/tesseract-ocr)
- [ Tesseract-OCR 字符识别---样本训练](http://blog.csdn.net/firehood_/article/details/8433077)
- [开源图文识别引擎实现验证码识别 ](http://bbs.aardio.com/forum.php?mod=viewthread&tid=12601)
- [Tesseract-OCR 3.02 训练笔记](http://www.cnblogs.com/ShineTan/archive/2013/04/15/3021523.html)
- [Tesseract 3 语言数据的训练方法](http://www.cnblogs.com/mjorcen/p/3799996.htm)
- [Tesseract 3.02中文字库训练](http://www.cnblogs.com/mjorcen/p/3800739.html)
- [Mac下Tesseract安装部署](http://holybless.iteye.com/blog/1338717)
- [基于Tesseract-OCR的名片识别系统](http://cdmd.cnki.com.cn/Article/CDMD-10561-1014065487.htm)
- [Github_Tesseract-OCR_For_PHP](https://github.com/thiagoalessio/tesseract-ocr-for-php)