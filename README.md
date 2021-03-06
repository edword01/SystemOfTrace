# SystemOfTrace
一个轨迹相似度分析系统－计算轨迹或图形的形状相似度

## 开发及测试环境:
GIS：百度地图  
主开发环境：MyEclipse2015  
JDK配置：JDK1.8  
数据库：MySQLServer 5.0  
服务器：apache-tomcat-8.0.32  
测试浏览器：FireFox 47.0

# 核心算法的具体实现:  
分析：在本系统中，由于轨迹点的绝对位置是要参与比较的，因此，针对针对每条轨迹的归一化是不需要的。  
步骤：  
    1.首先针对两个点，计算其转化距离，这里用空间位置上的欧几里得距离来代表，而对于带有时间的轨迹点求转化距离还需要带有时间跨度的计算，最后设置欧几里得距离和时间跨度的权值来获得两个点的转化距离  
    ![步骤1](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%872.png)  
    2.对于两条轨迹的比较，这里采用LCSS算法来求得两条轨迹的编辑距离，其中两条轨迹的长度是可以不同的，即所包含的轨迹点是不需要相同，而其中元素的转化惩罚值即为轨迹点的转化距离  
    ![步骤2](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%873.png)  
    3.最后，对于获得的两条轨迹的编辑距离，这里采用指数函数的方法，将最终的相似度作为编辑距离的底小于1的指数函数，满足当编辑距离为1时，相似度为1，即100%,而当编辑距离增大时,相似度减小，且减小的趋势越来越不明显，并且最终趋近于0，恰好符合相似度求解的要求。  
    ![步骤3](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%874.png)
# 数据库表设计:  
数据表总体分为两组，存放轨迹点数据的数据点表和存放各轨迹之间相似度的相似度表而每组又分为带时间和不带时间的数据表(下面均以带时间为例介绍):  
![数据表](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%878.png)  

点表：lon,lat分别表示点的经纬度点，time代表时间，sort_id表示每个点的主键，而id代表该店所属于的轨迹的id  
![点表](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%879.png)  

相似度表：id1和id2分别代表比较的两条轨迹，且两者组成组合主键，dist代表两条轨迹的编辑距离，之后可通过公式获得对应的相似度  
![相似度表](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%8710.png)  


# 界面设计:
界面模块:  
1.标题栏：
　　包含标题按钮－“轨迹相似度分析系统”和隐藏按钮  
2.菜单栏：
　　包含三个一级菜单－计算,查询,自定义轨迹和一个上传文件按钮  
  3.地图显示界面：
　　该主界面主要用于轨迹路径的显示和自定义轨迹的绘制  
  ![主界面](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%871.png)  
  


### 功能实现-计算:  
功能概述： 给定任意两条同类型（带时间或不带时间）的轨迹，计算出两者之间的相似度
功能实现效果：点击按钮后跳转至显示网页得到选定两条轨迹的相似度  
  ![计算](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%875.png)  
  
### 功能实现-查询:  
功能概述： 对于给定的一个对象轨迹，查找与其相似的所有轨迹，按照相似度从高到低进行排序。  
功能实现效果：点击按钮后跳转至显示网页得到相似轨迹的列表，通过链接可以在地图上显示该相似轨迹
  ![查询](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%877.png)  
  
### 功能实现-自定义绘制轨迹:  
该功能允许通过鼠标点击直接在地图界面上创建不带时间的轨迹路径，并且自动将轨迹保存于数据库中，之后可以将自定义绘制的轨迹以数据库中已有的轨迹进行相似度的计算，并且查询出数据库中与之较为相似的轨迹，按照相似度从高到底排列  
 ![自定义](https://raw.githubusercontent.com/qzq2514/ImageForGithubMakdown/master/SystemOfTrace/%E5%9B%BE%E7%89%876.png)  
 
