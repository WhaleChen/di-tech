# Di-tech record  

## 1. 预设步骤  

- 熟悉问题和数据  
- 考虑使用工具  
- 考虑解决方案  

### 1.1 主要问题：  
[赛题详情](http://research.xiaojukeji.com/competition/detail.action?competitionId=DiTech2016)   

给定过去一段时间该城市所有地区的订单信息，预测该城市某一地区下一时间段的供需缺口。  

- 指定区域：不同功能的区域左右了出行需求，会影响拥堵情况。根据POI表知道该区域主要功能；  
- 指定时间：该时间段的天气，可能左右出行方式，由weather表得知；该时间段该区域的拥堵情况，可能左右出行方式，可由traffic表可知；  

确定X和y：  

y：供需缺口gap，由demand－supply得到。指定区域和时间时，demand即order表中的记录数，supply即order表中driver非空（不等于null）的记录数。  

所以，y需要计算出来。（对数据进行预处理）

X：出发地（poi情况）和目的地（poi情况）、价格、time、拥堵情况、天气情况（天气、温度、pm25）

### 1.2 工具  

- 不要用windows下载解压数据，否则会对一大堆垃圾文件感到困惑。  
- 不要试图把数据搞到github上去，太蠢！（捂脸～）
- 所有数据文件都可以直接用sublime打开直接观看。  
- 用ipython notebook做主要阵地。   
- 这是一个时间预测问题，主要评价指标在于对新数据预测的能力，而非训练集拟合能力，所以要用sklearn库来解决。

### 1.3 解决方案  

#### 1.3.1 数据  

区域信息的两张表：`cluster_map`和`poi_data`和时间无关，是统一数据。  

`order_data`、`traffic_data`、`weather_data`这三种表以天为单位，1天1张表；   

 `weather_data`每5分钟1条数据；其他两种则不一定，根据实际情况确定。  

对前21天数据，是否应该采取像“天龙八部”一样的合并方式，用一个list合并一下？  

对于分布在不同类型表中的数据（X的众多组成），是否应该用数据库关联放到一张表里？  

poi这个复杂的因素，如何处理？  

区域：指定区域可以确定天气、拥堵等情况。那么跨区域接单要如何对待？噢，实际计算时以有没有人应答为标准。只要无人应答，即缺。不会去管是否有人跑满城来应答。

#### 1.3.2 模型  

终于谈到模型了，……

## 2. 问题纪录

### Q1: CParserError  

	poi_1 = pd.read_csv('season_1/training_data/poi_data/poi_data', delimiter='\t')
	
运行：  

	CParserError                              Traceback (most recent call last)
	<ipython-input-16-e879f2bf6cd7> in <module>()

解决：  

参考：[csv - Python Pandas Error tokenizing data - Stack Overflow](http://stackoverflow.com/questions/18039057/python-pandas-error-tokenizing-data)，还没解决。

