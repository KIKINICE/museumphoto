# 项目名称：博物馆拍照app
### 项目概括：为在博物馆的相片增加特效，如实时抠图，提取图片中的文本，拍摄属于博物馆的大片。
### 项目：app应用
| 发布日期   | 2019/12/03   |
| ---------- | ------------ |
| 项目名称   | 博物馆拍照app |
| 项目状态   | 已完成       |
| 项目负责人 | 黄琪琪       |
# PRD1 价值主张设计
## 一、加值宣言
加值宣言：
* 使用户的拍照突出主体，而达到用户想要的效果。
* 提供多种图片参考，用户可以通过改变背景而提高用户的体验，增加趣味性。
* 为用户屏蔽不必要的背景信息，避免用户隐私的泄露。

## 二、核心价值 
核心价值宣言：
当下拍照风靡各个平台，成为人们日常生活中不可或缺的展示途径。在摄影美化类app中，主要包括了照片美化类、特效相机和视频类三种，这三类都含有丰富的素材，满足用户在各种场景下的修图需求。一直以来，美图系列app长期占据拍照美化类应用榜单首位，形成了庞大的用户群。但我们发现只有拥有特色的美图类app才可以突出重围。因此博物馆拍照app就以实时抠图这一特色来寻求市场，以抠图api为基础，以丰富的素材为动力，希望在市场中占得一席之地。
* 关键词;实时抠像技术，智能识别，精准分割、提取文本

## 三、用户分析

### (1).用户痛点宣言：
* 用户想要拍照时，往往发现背景杂乱，往往因为背景照片显得杂乱。用户通过这款qpp可以直接实现人体的轮廓与背景区分，使得照片干净协调。
* 用户拍照时，不想暴露人体以外信息，抠图实现了隐藏背景信息。
* 拍照类app同质化问题严重。其他美图类app大同小异，没有自己的特色。
* 其他美图类app没有实时抠图功能，只能后期处理。

### (2).目标用户分析
* 年龄分布——年轻用户为主

![目标用户年龄分布](https://images.gitee.com/uploads/images/2020/0107/223815_c9d0cdc5_1648193.png)

* 职业分布——上班族更爱拍

![目标用户职业分布](https://images.gitee.com/uploads/images/2020/0107/223937_33dec258_1648193.png "屏幕截图.png")

* 目标用户喜爱的拍照主题——自拍是主流

![拍照主题](https://images.gitee.com/uploads/images/2020/0107/224030_290e0e6d_1648193.png "屏幕截图.png")

* 选择第三方拍照应用的原因——特效最关键

![喜爱原因](https://images.gitee.com/uploads/images/2020/0107/224212_17cc0a66_1648193.png "屏幕截图.png")

## 四、人工智能概率性

* 概率性定义：机器学习面临的未来的数据和将来的行为引发的结果是不确定的，概率理论提供了一个框架模型去针对这种不确定性。
* 应用：图像识别、图像分类
* 可能出现的问题：API无法识别人像，或者不能识别到第二、第三人

## 五、需求列表与人工智能API加值 


| 用户需求                                             | API  用户需求  |API参考链接|
| ---------------------------------------------------- | ----------------------- |----------------------- |
| 识别出物体的轮廓 | Face++的抠像SDK             |https://www.faceplusplus.com.cn/sdk/bodyoutline/|


# PRD2产品设计原型

## 一、原型设计
### 1.产品结构
![产品结构](https://images.gitee.com/uploads/images/2020/0110/010227_b112d831_1648193.png "屏幕截图.png")
### 2.原型
![原型](https://images.gitee.com/uploads/images/2020/0110/010413_3a9367fc_1648193.png "屏幕截图.png")
## 二、口头操作说明
用户下载app以后，用户同意app访问相机等权限后，可以选择各种背景样式，app识别出图像中物体的轮廓范围，与背景进行区分。还可以选择多种特效。实现手机拍博物馆拍大片的效果。
## 三、使用比较分析
* 百度图像分割API
  *  优点：支持返回分割后的二值图、灰度图、人像前景图，并可通过参数设置，自主设置返回哪些结果图，避免造成带宽浪费
  *  缺点：分割边沿粗糙
![baidu](https://images.gitee.com/uploads/images/2019/1211/032201_da3aa733_1648193.png "屏幕截图.png")
* Face++抠像SDK
  *  优点：分割边缘更加细致
  *  缺点：价格不明
*  价格对比
* Face++没有明确标价
![Face++](https://images.gitee.com/uploads/images/2019/1211/014710_5ecc1a1e_1648193.png "屏幕截图.png")
* 百度api明码实价
![Baidu produce](https://images.gitee.com/uploads/images/2019/1211/014757_01ccc019_1648193.png "屏幕截图.png")

## 四、API的输入及输出
### 提取文本的API输入
```
import requests
import time
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Polygon
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
if 'COMPUTER_VISION_SUBSCRIPTION_KEY' in os.environ:
    subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
else:
    print("\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

if 'COMPUTER_VISION_ENDPOINT' in os.environ:
    endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

text_recognition_url = endpoint + "vision/v2.0/read/core/asyncBatchAnalyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "[https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1569253082583&di=d31f33bdd0dfcd1cdb0d9ea6ebab5e97&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201508%2F25%2F20150825152104_mLYWU.thumb.1600_0.jpeg](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1569253082583&di=d31f33bdd0dfcd1cdb0d9ea6ebab5e97&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201508%2F25%2F20150825152104_mLYWU.thumb.1600_0.jpeg)"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
data = {'url': image_url}
response = requests.post(
    text_recognition_url, headers=headers, json=data)
response.raise_for_status()

# Extracting text requires two API calls: One call to submit the
# image for processing, the other to retrieve the text found in the image.

# Holds the URI used to retrieve the recognized text.
operation_url = response.headers["Operation-Location"]

# The recognized text isn't immediately available, so poll to wait for completion.
analysis = {}
poll = True
while (poll):
    response_final = requests.get(
        response.headers["Operation-Location"], headers=headers)
    analysis = response_final.json()
    print(analysis)
    time.sleep(1)
    if ("recognitionResults" in analysis):
        poll = False
    if ("status" in analysis and analysis['status'] == 'Failed'):
        poll = False

polygons = []
if ("recognitionResults" in analysis):
    # Extract the recognized text, with bounding boxes.
    polygons = [(line["boundingBox"], line["text"])
                for line in analysis["recognitionResults"][0]["lines"]]

# Display the image and overlay it with the extracted text.
plt.figure(figsize=(15, 15))
image = Image.open(BytesIO(requests.get(image_url).content))
ax = plt.imshow(image)
for polygon in polygons:
    vertices = [(polygon[0][i], polygon[0][i+1])
                for i in range(0, len(polygon[0]), 2)]
    text = polygon[1]
    patch = Polygon(vertices, closed=True, fill=False, linewidth=2, color='y')
    ax.axes.add_patch(patch)
    plt.text(vertices[0][0], vertices[0][1], text, fontsize=20, va="top")
```
      
### 提取文本的API输出
{'status': 'Running'}
{'status': 'Succeeded', 'recognitionResults': [{'page': 1, 'clockwiseOrientation': 2.48, 'width': 1600, 'height': 900, 'unit': 'pixel', 'lines': [{'boundingBox': [78, 144, 433, 187, 427, 315, 78, 303], 'text': 'you', 'words': [{'boundingBox': [88, 144, 384, 167, 372, 325, 76, 302], 'text': 'you', 'confidence': 'Low'}]}, {'boundingBox': [373, 208, 1412, 253, 1407, 360, 368, 315], 'text': 'can do that !', 'words': [{'boundingBox': [538, 219, 780, 227, 777, 334, 534, 323], 'text': 'can'}, {'boundingBox': [918, 233, 1030, 238, 1027, 345, 916, 341], 'text': 'do'}, {'boundingBox': [1121, 242, 1344, 253, 1343, 357, 1119, 349], 'text': 'that'}, {'boundingBox': [1364, 254, 1410, 257, 1409, 359, 1363, 358], 'text': '!'}]}, {'boundingBox': [362, 380, 1324, 386, 1323, 562, 360, 555], 'text': 'Try to do it !', 'words': [{'boundingBox': [409, 384, 678, 406, 686, 548, 417, 557], 'text': 'Try'}, {'boundingBox': [749, 410, 925, 418, 932, 539, 756, 546], 'text': 'to'}, {'boundingBox': [960, 419, 1090, 422, 1096, 533, 967, 538], 'text': 'do'}, {'boundingBox': [1125, 422, 1231, 422, 1236, 527, 1131, 531], 'text': 'it'}, {'boundingBox': [1266, 422, 1313, 422, 1318, 523, 1271, 525], 'text': '!'}]}]}]}
![图例](https://images.gitee.com/uploads/images/2020/0110/022800_6e525b4c_1648193.png "屏幕截图.png")

## 五、使用风险报告
* 可能会有图片不清晰的问题，需要用户重新提交图片
* 上传的物体照片的物体太过稀奇而无法分析
