# 疫情防控  

疫情防控小程序，方便学校对学生健康状况进行统计  
![疫情防控](http://120.79.54.89:8090/upload/2020/4/gh_4d456dd8fa8e_258-b07650e56f0d4985879788a766d19133.jpg "疫情防控.jpg")  

> 测试账号  
> 学号：20169999  
> 姓名：测试  

## 本项目具备以下特点：  
1.绑定个人信息，填写报表时无需再次填写  
2.区分教师、学生、辅导员、院长四类用户  
3.导出Excel表格，不同颜色标注发热人员   
  
## 依赖  
1.本项目使用云开发，无需域名，无需部署服务器  
2.导出Excel模块依赖exceljs库  

## 部署说明  
1.注册微信小程序  
2.使用微信开发者工具导入项目，修改AppId  
3.小程序后台创建订阅消息模板“打卡提醒”和“信息提交成功通知”（分别用于提醒学生打卡和提醒辅导员打卡已完成）  
![QQ截图20200330152647](https://images.gitee.com/uploads/images/2020/0405/080139_cf29d405_1694647.png)  
![QQ截图20200330152711](https://images.gitee.com/uploads/images/2020/0405/080139_b975245a_1694647.png)  
并修改小程序中对应id(和参数)：  
```javascript
// miniprogram/pages/checkin/checkin.js  

const tmpId = 'bzVdoPY3-KljJBAGe_vGlOz7PRUsGqlZITm85uKyUWk'  // 提醒学生消息模板  
const tmpIdt = 'E0u3PLO8OThB8pwGhwJn68ehfYzMcBltNjQPOoEAhYY'  // 提醒辅导员消息模板  
```
```javascript  
// 云函数文件remind/index.js  
```  
  
4.数据库创建，格式如下：  
> 注意：settings集合中数据id固定，请勿修改
- identi集合（用于身份验证）  
```javascript
//学生
{
  "userInfo":{
    "no":"xxxxxxx",
    "name":"xxx",
    "class_":"信16xx-3",
    "phone":"xxxxx",
    "department":"信息科学与技术学院",
    "type":"学生"
  }
}

//副院长、辅导员
{
  "userInfo": {
    "department": "信息科学与技术学院",
    "manage_classes": [
      "信xxxx-x",
      "信xxxx-x",
      "信xxxx-x"
    ],
    "name": "测试",
    "no": "20199999",
    "phone": "151xxxxx238",
    "type": "副院长"
  }
}

//教师
{
  "userInfo":{
    "no":"xxxxxxx",
    "name":"xxx",
    "phone":"xxxxx",
    "department":"信息科学与技术学院",
    "type":"教师"
  }
}
```
- settings集合（配置上报截止时间，注意：本条数据id固定，请勿修改）
```javascript
{
  "_id": "deadline",
  "hours": "11",
  "minutes": "00"
}
```
- classes集合（副院长身份用户会用到，注意：本条数据id固定，请勿修改）  
> 推荐在[https://www.json.cn/](https://www.json.cn/)查看json结构  
```javascript
{"_id":"class_set","department":["土木工程学院","机械工程学院","经济管理学院","文法学院","交通运输学院","建筑与艺术学院","材料科学与工程学院","电气与电子工程学院","信息科学与技术学院","工程力学系","数理系","外语系"],"major":{"土木工程学院":["中铁国际班","中铁建国际班（土建类）","土木工程(地下工程)","土木工程(建筑工程)","土木工程(桥梁工程)","土木工程(涉外工程)","因材土木(地下工程)","因材土木(建筑工程)","因材土木(桥梁工程)","因材土木(涉外工程)","因材土木(铁道工程)","土木工程(铁道工程)","安全工程","测绘工程","城市地下空间工程","勘查技术与工程","土木（卓越）","土木工程(工程试验班)","测绘工程(工程试验班)","铁道工程","土木工程","因材土木","土木工程(专接本)"],"机械工程学院":["机械设计制造及其自动化","机械设计制造及其自动化(工程机械)","机械设计制造及其自动化(机械电子工程)","机械设计制造及其自动化(机械设计与制造)","工业设计","测控技术与仪器","车辆工程","机械（卓越）","因材机械","因材机械(工程机械)","因材机械(机械电子工程)","因材机械(机械设计与制造)","建筑环境与能源应用工程","机械设计制造及其自动化（国际班）","建筑环境与能源应用工程(工程试验班)","机械电子工程","机械(专接本)"],"经济管理学院":["工程管理（双学位）","中铁建国际班（非土建类）","市场营销","工程管理","金融学","财务管理","会计学","信息管理与信息系统","国际经济与贸易","物流管理","电子商务","公共事业管理","会计学（CPA实验班）","工商管理类","工程管理(辅修学位)","会计学(专接本)","工程管理(专接本)","物流管理(专接本)"],"文法学院":["法学(双学位)","法学","汉语言文学","汉语言文学(双学位)","法学(辅修学位)","汉语言文学(辅修学位)"],"交通运输学院":["交通运输（卓越）","交通工程(轨道交通工程)","交通工程(城市交通工程)","交通运输","交通工程(公路交通工程)","交通运输类"],"建筑与艺术学院":["建筑学","视觉传达设计","环境设计"],"材料科学与工程学院":["无机非金属材料工程(无机非金属材料工程)","无机非金属材料工程(无机非金属材料科学)","金属材料工程(金属材料)","金属材料工程(焊接技术与工程)","材料科学与工程","功能材料","材料类"],"电气与电子工程学院":["电气工程及其自动化","电子信息工程","通信工程","因材电气","电气(卓越)","自动化","轨道交通信号与控制","电气工程及其自动化(工程试验班)","自动化类","电气(专接本)","自动化(专接本)"],"信息科学与技术学院":["计算机科学与技术","网络工程","教育技术学","信息工程","软件工程","数字媒体技术","计算机类"],"工程力学系":["工程力学"],"数理系":["数学与应用数学","应用物理学"],"外语系":["英语（双学位）","英语","小语种班","英语(辅修学位)"]},"class_":{"中铁国际班":["中铁国际班15","中铁国际班16"],"中铁建国际班（土建类）":["中铁建国际1班","中铁建国际班16-1"],"土木工程(地下工程)":["地下16-1","地下16-2","地下17-1","地下17-2"],"土木工程(建筑工程)":["建工16-1","建工16-2","建工17-1","建工17-2"],"土木工程(桥梁工程)":["桥梁16-1","桥梁16-2","桥梁16-3","桥梁16-4","桥梁16-5","桥梁16-6","桥梁16-7","桥梁16-8","桥梁16-9","桥梁17-1","桥梁17-2","桥梁17-3","桥梁17-4","桥梁17-5","桥梁17-6"],"土木工程(涉外工程)":["涉外16","涉外17"],"因材土木(地下工程)":["试16地下","试地下17"],"因材土木(建筑工程)":["试16建工","试建工17"],"因材土木(桥梁工程)":["试16桥梁","试桥梁17"],"因材土木(涉外工程)":["试16涉外"],"因材土木(铁道工程)":["试16铁道","试铁道17"],"土木工程(铁道工程)":["铁道16-1","铁道16-2","铁道16-3","铁道16-4","铁道17-1","铁道17-2","铁道17-3","铁道17-4"],"安全工程":["土1602-1","土1602-2","土1702-1","土1702-2","土1802-1","土1802-2","土1902-1","土1902-2","土1902-3","土1902-4"],"测绘工程":["土1603-1","土1603-2","土1603-3","土1703-1","土1703-2","土1703-3","土1803-1","土1803-2","土1803-3","土1803-4","土1903-1","土1903-2","土1903-3","土1903-4"],"城市地下空间工程":["土1605-1","土1605-2","土1705-1","土1705-2","土1805-1","土1805-2","土1905-1","土1905-2"],"勘查技术与工程":["土1604-1","土1604-2","土1704-1","土1704-2","土1804-1","土1804-2","土1904-1","土1904-2"],"土木（卓越）":["试1605","试1705","土18卓越","土19卓越"],"土木工程(工程试验班)":["工1601","工1701","工1801","工1901"],"测绘工程(工程试验班)":["工1602","工1702","工1802","工1902"],"铁道工程":["土1706-1","土1706-2","土1806-1","土1806-2","土1906-1","土1906-2"],"土木工程":["土1801-1","土1801-2","土1801-3","土1801-4","土1801-5","土1801-6","土1801-7","土1801-8","土1801-9","土1801-10","土1801-11","土1801-12","土1801-13","土1801-14","土1901-1","土1901-2","土1901-3","土1901-4","土1901-5","土1901-6","土1901-7","土1901-8","土1901-9","土1901-10","土1901-11","土1901-12","土1901-13","土1901-14","土1901-15"],"因材土木":["土18詹天佑","土18茅以升","土19詹天佑","土19茅以升"],"土木工程(专接本)":["土1901z-1","土1901z-2","土1901z-3"],"机械设计制造及其自动化":["机1601-1","机1601-2","机1601-3","机1601-4","机1601-5","机1601-6","机1701-1","机1701-2","机1701-3","机1801-1","机1801-2","机1801-3","机1901-1","机1901-2","机1901-3","机1901-4","机1901-5"],"机械设计制造及其自动化(工程机械)":["16工程机械"],"机械设计制造及其自动化(机械电子工程)":["16机电"],"机械设计制造及其自动化(机械设计与制造)":["16机设"],"工业设计":["机1603","机1703","机1803","机1903-1","机1903-2"],"测控技术与仪器":["机1604","机1704","机1804","机1904-1","机1904-2"],"车辆工程":["机1605-1","机1605-2","机1705-1","机1705-2","机1805-1","机1805-2","机1905-1","机1905-2","机1905-3"],"机械（卓越）":["试1606","试1706","机18卓越","机19卓越"],"因材机械":["试1603","试1703","机18茅以升","机19茅以升"],"因材机械(工程机械)":["试16工程机械"],"因材机械(机械电子工程)":["试16机电"],"因材机械(机械设计与制造)":["试16机设"],"建筑环境与能源应用工程":["机1602-1","机1602-2","机1602-3","机1702-1","机1702-2","机1702-3","机1802-1","机1802-2","机1802-3","机1902-1","机1902-2","机1902-3"],"机械设计制造及其自动化（国际班）":["机1601-外1","机1601-外2","机1701-外1","机1701-外2","机1801-外1","机1801-外2","机1901-外1","机1901-外2"],"建筑环境与能源应用工程(工程试验班)":["工1604","工1704","工1804","工1904"],"机械电子工程":["机1710-1","机1710-2","机1810-1","机1810-2","机1910-1","机1910-2","机1910-3"],"机械(专接本)":["机1901z-1","机1901z-2"],"工程管理（双学位）":["双15工管","双16工管","双17工管"],"中铁建国际班（非土建类）":["中铁建国际2班","中铁建国际班16-2"],"市场营销":["经1601","经1701","经1801","经1901"],"工程管理":["经1602-1","经1602-2","经1602-3","经1602-4","经1602-5","经1702-1","经1702-2","经1702-3","经1802-1","经1802-2","经1802-3","经1902-1","经1902-2","经1902-3","经1902-4"],"金融学":["经1603","经1703","经1803-1","经1803-2","经1903-1","经1903-2"],"财务管理":["经1604-1","经1604-2","经1704-1","经1704-2","经1704-3"],"会计学":["经1605-1","经1605-2","经1605-3","经1605-4","经1705"],"信息管理与信息系统":["经1606","经1706","经1806","经1906-1","经1906-2"],"国际经济与贸易":["经1607","经1707","经1807","经1907"],"物流管理":["经1608-1","经1608-2","经1608-3","经1608-4","经1708-1","经1708-2","经1708-3","经1708-4","经1808-1","经1808-2","经1808-3","经1808-4","经1908-1","经1908-2","经1908-3","经1908-4"],"电子商务":["经1609","经1709","经1809"],"公共事业管理":["经1610","经1710","经1810"],"会计学（CPA实验班）":["注会1605","注会1705"],"工商管理类":["经1811-1","经1811-2","经1811-3","经1811-4","经1811-5","经1911-1","经1911-2","经1911-3","经1911-4","经1911-5","经1911-6"],"工程管理(辅修学位)":["辅18工管"],"会计学(专接本)":["经1905z-1","经1905z-2"],"工程管理(专接本)":["经1902z-1","经1902z-2","经1902z-3"],"物流管理(专接本)":["经1908z"],"法学(双学位)":["双15法学","双16法学","双17法学"],"法学":["文1601-1","文1601-2","文1701-1","文1701-2","文1801-1","文1801-2","文1901-1","文1901-2"],"汉语言文学":["文1604-1","文1604-2","文1704-1","文1704-2","文1804-1","文1804-2","文1904-1","文1904-2"],"汉语言文学(双学位)":["双17中文"],"法学(辅修学位)":["辅18法学"],"汉语言文学(辅修学位)":["辅18中文"],"交通运输（卓越）":["试1607","试1707","交18卓越","交19卓越"],"交通工程(轨道交通工程)":["交1601-1","交1601-2","交1701-1","交1701-2","交1701-3"],"交通工程(城市交通工程)":["交1601-3","交1601-4","交1701-4"],"交通运输":["交1602-1","交1602-2","交1602-3","交1602-4","交1602-5","交1602-6","交1702-1","交1702-2","交1702-3","交1702-4","交1702-5","交1702-6","交1702-7"],"交通工程(公路交通工程)":["交1701-5"],"交通运输类":["交1803-1","交1803-2","交1803-3","交1803-4","交1803-5","交1803-6","交1803-7","交1803-8","交1903-1","交1903-2","交1903-3","交1903-4","交1903-5","交1903-6","交1903-7","交1903-8"],"建筑学":["建1401-1","建1401-2","建1401-3","建1401-4","建1501-1","建1501-2","建1501-3","建1501-4","建1601-1","建1601-2","建1601-3","建1601-4","建1701-1","建1701-2","建1701-3","建1701-4","建1801-1","建1801-2","建1801-3","建1801-4","建1901-1","建1901-2","建1901-3","建1901-4"],"视觉传达设计":["建1603-1","建1603-2","建1703-1","建1703-2","建1803-1","建1803-2","建1903-1","建1903-2"],"环境设计":["建1602-1","建1602-2","建1702-1","建1702-2","建1802-1","建1802-2","建1902-1","建1902-2"],"无机非金属材料工程(无机非金属材料工程)":["材1601-1","材1601-2","材1701-1","材1701-2","材1801-1","材1801-2","材1801-3"],"无机非金属材料工程(无机非金属材料科学)":["材1601-3","材1701-3","材1801-4"],"金属材料工程(金属材料)":["材1602-1","材1702-1","材1802-1"],"金属材料工程(焊接技术与工程)":["材1602-2","材1602-3","材1602-4","材1702-2","材1702-3","材1702-4","材1802-2","材1802-3","材1802-4"],"材料科学与工程":["材1603-1","材1603-2","材1703-1","材1703-2","材1803-1","材1803-2"],"功能材料":["材1604","材1704","材1804"],"材料类":["材1805-1","材1805-2","材1805-3","材1805-4","材1805-5","材1805-6","材1805-7","材1805-8","材1805-9","材1905-1","材1905-2","材1905-3","材1905-4","材1905-5","材1905-6","材1905-7","材1905-8","材1905-9","材1905-10"],"电气工程及其自动化":["电1601-1","电1601-2","电1601-3","电1601-4","电1601-5","电1701-1","电1701-2","电1701-3","电1801-1","电1801-2","电1801-3","电1901-1","电1901-2","电1901-3"],"电子信息工程":["电1604-1","电1604-2","电1704-1","电1704-2","电1804-1","电1804-2","电1904-1","电1904-2"],"通信工程":["电1605-1","电1605-2","电1705-1","电1705-2","电1805-1","电1805-2","电1905-1","电1905-2"],"因材电气":["试1604","试1704","电18茅以升","电19茅以升"],"电气(卓越)":["试1608","试1708","电18卓越","电19卓越"],"自动化":["电1602-1","电1602-2","电1702-1","电1702-2","电1702-3"],"轨道交通信号与控制":["电1607-1","电1607-2","电1607-3","电1707-1"],"电气工程及其自动化(工程试验班)":["工1603","工1703","工1803","工1903"],"自动化类":["电1806-1","电1806-2","电1806-3","电1806-4","电1906-1","电1906-2","电1906-3","电1906-4","电1906-5"],"电气(专接本)":["电1901z-1","电1901z-2"],"自动化(专接本)":["电1902z-1"],"计算机科学与技术":["信1601-1","信1601-2","信1601-3","信1701-1","信1701-2","信1701-3","信1801-1","信1801-2","信1801-3"],"网络工程":["信1603","信1703","信1803"],"教育技术学":["信1602","信1702","信1802","信1902"],"信息工程":["信1604-1","信1604-2","信1704-1","信1704-2","信1804-1","信1804-2","信1904-1","信1904-2","信1904-3"],"软件工程":["信1605-1","信1605-2","信1605-3","信1705-1","信1705-2","信1705-3","信1805-1","信1805-2","信1805-3"],"数字媒体技术":["信1606","信1706","信1806"],"计算机类":["信1907-1","信1907-2","信1907-3","信1907-4","信1907-5","信1907-6","信1907-7","信1907-8","信1907-9"],"工程力学":["力1601-1","力1601-2","力1601-3","力1601-4","力1701-1","力1701-2","力1701-3","力1701-4","力1801-1","力1801-2","力1801-3","力1801-4","力1901-1","力1901-2","力1901-3","力1901-4"],"数学与应用数学":["数1601-1","数1601-2","数1701-1","数1701-2","数1801-1","数1801-2","数1901-1","数1901-2"],"应用物理学":["数1602-1","数1602-2","数1702-1","数1702-2","数1802-1","数1802-2","数1902-1","数1902-2"],"英语（双学位）":["双15英语-1","双15英语-2","双16英语-1","双16英语-2","双17英语-1","双17英语-2"],"英语":["外1601-1","外1601-2","外1701-1","外1701-2","外1801-1","外1801-2","外1901-1","外1901-2"],"小语种班":["16小语种","17小语种","18小语种-俄语","18小语种-日语","19小语种"],"英语(辅修学位)":["辅18英语-1","辅18英语-2"]}}
```   
- users集合（只需创建，无需手动添加数据）  

5.百度地图API配置：  
请参考：  
[微信小程序JavaScript API](http://lbsyun.baidu.com/index.php?title=wxjsapi)  
在小程序pages/form/form.js，getLocation函数中替换ak:  
```javascript
//pages/form/form.js
//  getLocation()
    //…………
          data: {
            ak: 'qwL1G59s11CGqoB9rOX3irKNswbSCqPp', //替换此处ak
            location: `${res.latitude},${res.longitude}`,
            output: 'json'
          },
    //…………
```
## 开发人员  
宁宇 李志强 王世博  
  
## Bug反馈  
[反馈/建议](http://120.79.54.89:8090/archives/%E9%93%81%E5%A4%A7%E9%98%B2%E6%8E%A7%E5%8F%8D%E9%A6%88)  
或 iv2013@qq.com  
或 QQ：603631695  
