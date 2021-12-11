燃尽图：

![image-20211130141906380](C:\Users\86135\Desktop\Java学习笔记（待发博客）\image-20211130141906380.png)





er:

![image-20211130141935173](C:\Users\86135\AppData\Roaming\Typora\typora-user-images\image-20211130141935173.png)



数据库说明：

1. 语法: wx.setStorage() || wx.setStorageSync() || .....
2. 注意点： 
   1. 建议存储的数据为json数据
   2. 单个 key 允许存储的最大数据长度为 1MB，所有数据存储上限为 10MB
   3. 属于永久存储，同H5的localStorage一样