YOLOv11 目标检测 API 文档
概述
本API提供基于YOLOv11模型的目标检测服务。用户可以上传单张图片或批量上传图片（Base64编码）进行物体识别，并获取检测到的物体类别、置信度及边界框坐标。
基本信息
● API 基地址 (Base URL): http://202.112.157.13:5000
● 请求与响应格式: 所有请求体和响应体均为 JSON 格式，除非特别说明（如文件上传）。
● 坐标格式: 边界框 (bbox) 坐标格式为 [x1, y1, x2, y2]，分别代表左上角点的x、y坐标和右下角点的x、y坐标。
端点 (Endpoints)
1. 健康检查
● 路径: /health
● 描述: 检查API服务是否正常运行，以及模型和GPU状态。
● 成功响应 (200 OK):
{
  "status": "healthy",
  "gpu": true  // 如果GPU可用则为true，否则为false
}
● 失败响应 (503 Service Unavailable，若模型加载失败):
{
  "status": "unhealthy",
  "message": "模型加载失败"
}
● 示例 (curl):
curl http://202.112.157.13:5000/health
2. 单张图片目标检测
● 路径: /detect
● 方法: POST
● 描述: 上传一张图片进行目标检测。
● 请求参数 (Query Parameters):
  ○ conf (可选, float): 置信度阈值，介于0.0到1.0之间。只有置信度高于此值的检测结果才会被返回。默认值为 0.5。
● 请求体:
  ○ 方式一：文件上传 (Content-Type: multipart/form-data)
    ■ 表单字段名: image
    ■ 值: 图像文件 (例如：.jpg, .png)
  ○ 方式二：Base64编码图像 (Content-Type: application/json)
{
  "image": "<base64_encoded_image_string>"
}
● 成功响应 (200 OK):
{
  "count": 1, // 检测到的目标数量
  "detections": [
    {
      "class": 0,                             // 物体类别ID
      "class_name": "Kongdong",               // 物体类别名称
      "confidence": 0.8113518953323364,      // 置信度
      "bbox": [94.52, 887.16, 2388.87, 1399.28] // 边界框 [x1, y1, x2, y2]
    }
    // ... 可能有更多检测结果
  ]
}
● 错误响应:
  ○ 400 Bad Request:
{"error": "没有提供图像"}
// 或
{"error": "Base64图像解码失败: <具体错误信息>"}
  ○ 500 Internal Server Error:
{"error": "模型未正确加载"}
// 或
{"error": "<检测过程中的具体错误信息>"}

● 示例 (curl):
  ○ 文件上传:
curl -X POST "http://202.112.157.13:5000/detect?conf=0.6" -F "image=@/path/to/your/image.jpg"
  ○ Base64 JSON (示例，实际Base64字符串会很长):
curl -X POST -H "Content-Type: application/json" \
-d '{"image": "iVBORw0KGgoAAAANSUhEUgAAAAUA..."}' \
"http://202.112.157.13:5000/detect?conf=0.6"


3. 批量图片目标检测 (Base64)
● 路径: /batch_detect
● 方法: POST
● 描述: 上传一个包含多张Base64编码图像的列表进行批量目标检测。
● 请求参数 (Query Parameters):
  ○ conf (可选, float): 置信度阈值，应用于所有图像。默认值为 0.5。
● 请求体 (Content-Type: application/json):
{
  "images": [
    "<base64_encoded_image1_string>",
    "<base64_encoded_image2_string>"
    // ... 更多Base64编码的图像字符串
  ]
}
● 成功响应 (200 OK):
{
  "results": [
    { // 第一张图片的结果
      "status": "success",
      "count": 1,
      "detections": [
        {
          "class": 0,
          "class_name": "Kongdong",
          "confidence": 0.8113518953323364,
          "bbox": [94.52, 887.16, 2388.87, 1399.28]
        }
      ]
    },
    { // 第二张图片的结果 (如果处理失败)
      "status": "error",
      "error": "<针对该图片的具体错误信息>"
    }
    // ... 更多图片的结果
  ]
}
● 错误响应:
  ○ 400 Bad Request:
{"error": "没有提供图像列表"}
  ○ 500 Internal Server Error:
{"error": "模型未正确加载"}
// 或
{"error": "<批量检测过程中的具体错误信息>"}
● 示例 (curl, 实际Base64字符串会很长):
curl -X POST -H "Content-Type: application/json" \
-d '{"images": ["iVBORw0KGgoAAAAN...", "R0lGODlhAQABAIAAAAUEBAAAACwAAAAAAQABAAACAkQBADs="]}' \
"http://202.112.157.13:5000/batch_detect?conf=0.7"

类别说明 (Class Names and IDs)
类别ID (class)	类别名称 (class_name)
0	Kongdong (空洞)
1	Gangjin (钢筋)
2	ganggongjia（钢拱架）


