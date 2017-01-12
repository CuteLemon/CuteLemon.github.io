清理地图数据并导入MongoDB。

# Wrangle OpenStreetMap Data
Map Area: Wuhan,China

## 数据集存在的问题

1.中英文 存在英文结尾或拼音结尾的情况 如：'洪山大道 HongShan Road','黄州区 (Huangzhou)'

2.电话号码格式不一致 如：+86 027-863712348888,86 159 8664 9760,027 2698 8042

### 中英文


去掉了名称中的英文/拼音结尾。

### 电话号码
统一为：
座机： 12345678
手机： 88812345678

## 数据概览

### 文件大小
1. map_Wuhan.xml 75MB
2. map_Wuhan.xml.json 75MB

### 结点数量


```python
DB.find({'info.user':{'$exists':1}}).count()
```




    393368



### 道路数量


```python
DB.find({'type':'way'}).count()
```




    35150



### 用户数量


```python
len(DB.distinct('info.user'))
```




    432



### Top10 贡献者


```python
Top10 = DB.aggregate([{'$group':{'_id':'$info.user','count':{'$sum':1}}},
                     {'$sort':{'count':-1}},
                     {'$limit':10}])
for x in Top10:
    for y in x:
        print x[y],'\t',
    print '\n'
```

    124865 	GeoSUN 	

    49100 	Soub 	

    35956 	katpatuka 	

    29003 	jamesks 	

    18544 	Gao xioix 	

    14001 	dword1511 	

    8529 	hanchao 	

    6380 	keepcalmandmapon 	

    5988 	nuklearerWintersturm 	

    5938 	_kendell 	



### 年度更新量



```python
activeYear = DB.aggregate([{'$project':{'year':{'$substr':['$info.timestamp',0,4]}}},
                          {'$group':{'_id':'$year','count':{'$sum':1}}},
                          {'$sort':{'count':-1}}])
for x in activeYear:
    for y in x:
        print x[y],'\t',
    print '\n'
```

    100209 	2011 	

    77101 	2013 	

    72077 	2016 	

    62534 	2012 	

    30208 	2015 	

    26340 	2014 	

    16886 	2010 	

    4876 	2009 	

    3137 	2008 	




```python
prosperous_ave = DB.aggregate([{'$group':{'_id':'$address.street','count':{'$sum':1}}},
                              {'$sort':{'count':-1}},
                              {'$limit':3}])
for x in prosperous_ave:
    for y in x:
        print x[y],'\t',
    print '\n'
```

    393315 	None 	

    5 	桂中路 	

    4 	关山大道 	



### 其他统计

学校


```python
school = DB.find({'amenity':'school'})

for x in school:
    if 'name' in x:
        print x['name'],'\t',
        print '\n'
```

    育才中学 	

    解放中学 	

    华中农业大学附属学校 	

    华中师范大学第一附属中学 	

    中南民族大学 	

    马房山中学 	

    枫叶寄宿学校 	

    武汉大学第一附属小学 	

    武汉大学附属中学 	

    街道口小学 	

    广埠屯小学 	

    武汉电力职业技术学院 	

    民主路小学 	

    中华路小学 	

    武汉市洪山中学 	

    华中师范大学附属小学 	

    武汉市第十九初级中学 	

    武汉外校江南分校 	

    卓刀泉中学 	

    黄州西湖中学 	

    新华电脑学校 	

    警官职业学院 	

    武汉工程大学邮电与信息工程学院 	

    江汉区大夹街小学 	

    江汉区福建街小学 	

    武汉市第52中学 	

    江汉区大兴路小学 	

    汉口回民小学 	

    武汉市第七中学 	

    黄陂街小学分校 	

    武汉市人民中学 	

    江汉区武汉关小学 	

    江汉区清芬路小学 	

    武汉市第51中学 	

    硚口区板厂街小学 	

    硚口区新安街小学 	

    硚口区人和街小学 	

    武汉市财经学校 	

    硚口区安徽街小学 	

    硚口区宝善街小学 	

    硚口区星火小学 	

    武汉市第十六中学 	

    硚口区南阳村小学 	

    硚口区利济路小学 	

    武汉市第29中学 	

    硚口区红旗村小学 	

    武汉市第27中学 	

    硚口区游艺路小学 	

    硚口区跃进村小学 	

    硚口区六角亭小学 	

    江汉区天一街小学 	

    武汉市第五中学 	

    武汉市第75中学 	

    江汉区友谊路中学 	

    江汉区惠康里小学 	

    武汉市旅游学校 	

    江汉区前进二路小学 	

    武汉市第一中学 	

    江汉区桃源坊小学 	

    武汉市第19中学 	

    江岸区铭新街小学 	

    武汉市第21中学 	

    江岸区鄱阳街小学 	

    武汉市第20中学 	

    武汉市实验初级中学 	

    武汉市实验学校 	

    江岸区一元路小学 	

    江岸区黄陂路小学 	

    武汉市美术学校 	

    武汉市警予中学 	

    江岸区瑞祥路小学 	

    武汉市第16中学 	

    武汉市七一华源中学 	

    江岸区沈阳路小学 	

    武汉市第41中学 	

    江岸区四唯路小学 	

    武汉二中广雅中学 	

    江岸区长春街小学 	

    武汉市第二中学 	

    武汉市财贸学校 	

    江岸区吕锡三小学 	

    江汉区展览馆小学 	

    江汉区单洞新村小学 	

    江汉区江汉北路小学 	

    武汉市第28中学 	

    武汉市第12中学 	

    江汉区取水楼小学 	

    硚口区大通巷小学 	

    武汉市第64中学 	

    武汉市第64中学分校 	

    硚口区中山巷小学 	

    硚口区义烈巷小学 	

    硚口区庆新小学 	

    武汉市第56中学 	

    硚口区建乐村小学 	

    硚口区井岗山小学 	

    武汉市第一职业教育中心 	

    硚口区幸福村小学 	

    硚口区利济北路小学 	

    武汉市第62中学 	

    华中科技大学同济医院附属卫生学校 	

    武汉市第11中学 	

    硚口区崇仁路小学 	

    硚口区长征小学 	

    硚口区仁寿路小学 	

    武汉市第26中学 	

    武汉市第63中学 	

    硚口区水厂路小学 	

    武汉市第17初级中学 	

    水厂路中学 	

    硚口区太平洋小学 	

    武汉市第17中学 	

    硚口区新合村小学 	

    硚口区常码头小学 	

    江汉区滑坡路小学 	

    江汉区万松园小学 	

    江汉区卫星村小学 	

    武汉市第12初级中学 	

    江汉区航空路小学 	

    江岸区台北路小学 	

    红安县马井中学 	

    毛岗小学 	

    Sanjiaohu 	

    硚口区易家墩小学 	

    武汉市晒湖中学 	

    梅苑学校小学部 	

    华中师范大学附属万科金色城市小学 	

    武汉市第三十八中学 	

    南湖一小（北校区） 	

    武汉市洪山高中 	

    武汉体育学院运动学校 	

    武汉市光谷第三小学 	

    武汉市光谷第二高级中学 	

    武汉市交通学校 	

    武汉思远IT教育 	

    长江大学 	

    葛店高中 	

    武汉市青菱中学 	

    武汉市第十三中学 	

    金马中学 	

    湖北第二师范学院南大门 	

    湖北第二师范学院北门 	

    湖北第二师范学院南二门 	

    武汉大学（正门牌坊） 	

    实验楼 	

    中南财经政法大学北门 	

    Libray 	

    华科附中 	

    华科附小 	

    水果湖第一中学 	

    水果湖高级中学 	

    水果湖第二小学 	

    武汉市第二中学 	

    华科幼儿园 	

    新东方 	

    武汉市第十五中学 	

    武珞路初级中学 	

    华科附中 	

    东区幼儿园 	

    东区附小 	

    鲁巷小学 	

    鲁巷中学 	

    翠微中学 	

    钟家村小学 	

    武汉市第三高级中学 	

    武汉市翠微路小学 	

    武汉市翠微中学赫山校区 	

    玫瑰园小学 	

    德才小学 	

    武汉市德才中学 	

    武汉市第三中学四新分校 	

    十里铺小学 	

    武汉市七里中学 	

    湖北省委党校 	

    Teaching 	

    Arts 	

    Laboratory 	

    Corridor 	

    Teaching 	

    Corridor 	

    Corridor 	

    Corridor 	

    Corridor 	

    武汉市第14中学 	

    青山区钢城第二小学 	

    华中师范大学附属万科金色城市小学 	

    武汉市白沙洲中学 	

    汉川市第二高级中学 	

    汉川市马口镇高湖小学 	

    武汉市光谷第二小学 	

    武汉市光谷第六小学 	

    武汉市光谷第一小学 	

    武汉市光谷实验中学 	

    武汉市光谷第二初级中学 	

    武汉市光谷第三小学北校区 	

    武钢三中 	

    武汉市一中 	

    南湖中学（南校区） 	

    湖北省实验中学 	

    南湖中学（北校区） 	

    武汉大学第二附属小学 	

    和平小学 	

    中南财经政法大学 	



餐馆


```python
print DB.find({'amenity':'restaurant'}).count()
```

    149


图书馆


```python
print DB.find({'amenity':'library'}).count()
```

    25


ATM


```python
print DB.find({'amenity':'atm'}).count()
```

    27


## 总结

OpenStreeMap是一个内容自由且允许所有人编辑的地图。由于对数据的约束较少，存在以下问题：


- 完整性。从节点的统计结果来看，学校比餐馆多，这不符合实际情况。这可能是因为餐馆的更新较快，人们不太愿意标注。也有可能是因为OpenStreetMap在武汉地区的贡献者较少，导致地图目前的粒度还很大，许多细节并不能体现。
-  一致性。电话号码格式以及中英文夹杂的情况也有出现。此种情况通过程序进行了判断并且纠正。
- 可靠性。由于用户的GPS精度并不能够保证，还有一些比如恶作剧似的节点如下：tag k="name" v="Path from my home to my friend's home"。这些节点难以靠程序找出并且纠正。

额外改进建议：

- 将xml文件整理成：中文名称-英文名称的列表，找出问题的节点，然后通过文本搜索进行替换。此种方法可以对数据集进行预处理，避免一些用户上传数据的错误，比如将建筑的属性标错了，以及本该是中文名称的地方写成了英文等等。此种方法需要耗费大量的时间，或许并不经济。
- 删除贡献数量少的用户数据。10%的用户贡献了90%的数据。这些用户的数据可能拥有更高的一致性和可靠性。不利的地方是可用的数据变少了，数据集的完整性会更差。



## 代码清单


```python
# -*- coding: utf-8 -*-
import xml.etree.cElementTree as ET
import pprint
import re
import codecs
import json

INFO = ['version','changeset','timestamp','user','uid']
TAG_Dict = {'addr:street':'street','addr:housenumber':'housenumber'}

# use for: Chinese name + English name,such as '关山大道 GuanShan Road'
# catch the English words
NAME_re = re.compile('\s.*\w')

# used to delete the non-num char
p = re.compile('\s+|-+|\++')

PHONE_re = re.compile('(86)?0?(27)?\d{8}$')
MOBILE_re = re.compile('(86)?\d{11}$')

# read the osm(xml) element and change it to python dictionary
def shape_element(element):
    node = {}
    if element.tag == 'node':
        node['type'] = 'node'

#         GPS data
        lat = element.attrib['lat']
        lon = element.attrib['lon']
        a = [float(lat),float(lon)]
        node['pos'] = a

        info = {}
        for x in INFO:
            info[x] = element.attrib[x]
        node['info'] = info
#         get the child tag info
        child = element.getchildren()
        for x in child:
            node[x.attrib['k']] = x.attrib['v']

    if element.tag == 'way':
        node['type'] = 'way'

        info = {}
        for x in INFO:
            info[x]=element.attrib[x]
        node['info']= info

        child = element.getchildren()
        ndlist = []
        address = {}
        for x in child:
            if x.tag == 'nd':
                ndlist.append(x.attrib['ref'])
                node['node_refs'] = ndlist
            if x.tag =='tag':
                if x.attrib['k']=='addr:street':
                    address['street']=x.attrib['v']
                if x.attrib['k']=='addr:housenumber':
                    address['housenumber']=x.attrib['v']
#                 if x.attrib['k'] in TAG_Dict:
#                     v = TAG_Dict[x.attrib['k']]
#                     address[v] = x.attrib['v']
                node[x.attrib['k']]=x.attrib['v']
                if address:
                    node['address'] = address
    return node

# clean the name field
def update_name(x):
    m = NAME_re.search(x)
    if m:
        return x[:m.start()]
    else:
        return x


# clean the number field
def update_number(x):
#     delete the non-num char such as +,
    new = re.sub(p,'',x)
    m = PHONE_re.match(new)
    if m:
        return new[-8:]
    m2 = MOBILE_re.match(new)
    if m2:
        return new[-11:]
    return 'INVALID'


def process_map(file_in,pretty = False):
    file_out = '{0}.json'.format(file_in)
    data = []
    with codecs.open(file_out,'w') as fo:
        for _,element in ET.iterparse(file_in):
            el = shape_element(element)
            if 'name' in el:
                el['name'] = update_name(el['name'])
            if 'address' in el:
                if 'street' in el['address']:
                    el['address']['street'] = update_name(el['address']['street'])
            if el:
                data.append(el)
                if pretty:
                    fo.write(json.dumps(el,indent=2)+'\n')
                else:
                    fo.write(json.dumps(el)+ '\n')
    return data

# transform the xml to json
data = process_map('map_Wuhan.xml')

# insert the json data to MongoDB
from pymongo import MongoClient
import json
client = MongoClient('mongodb://localhost:27017')
db = client.examples

def insert_data(data,db):
    for x in data:
        db.Wuhan.insert_one(x)

json_file = open('map_Wuhan.xml.json')
data = json_file.read().split('\n')
data.pop()
# delete the last''

for x in data:
    a = json.loads(x)
    db.Wuhan.insert_one(a)

json_file.close()


DB = db.Wuhan

```
