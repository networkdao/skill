name: aliyun-ecs-price-estimator
version: "1.0"
description: >-
  计算阿里云云主机（ECS）多类型配置的年度预估费用。
domain: cloud
subdomain: pricing
license: Apache-2.0
author: wangzl
tags: [aliyun, ecs, pricing, automation, cloud]

When to Use:
  当需要根据用户输入的多种 ECS 配置，自动从阿里云官网获取价格并计算总费用时。

Inputs:
  region (string): 区域，支持“华北（青岛）”、“华北（北京）”、“华北（张家口）”、“华北（呼和浩特）”。
  architecture (string): 架构，固定为“x86”。
  configs (array): 每种主机类型的配置数组，每项包含：
    - cpu (int): CPU 核心数
    - memory (int): 内存（GB）
    - system_disk (int, optional): 系统盘大小，默认50GB
    - data_disks (array, optional): 数据盘配置数组，每项包含容量（GB）和数量
    - count (int): 该类型主机数量

Workflow:
1. 遍历 configs 数组，对每种类型：
   a. 组装请求参数，调用 https://www.aliyun.com/price/product 选择"云服务器ECS",选择地域； 输入vCPU 数量，选择内存，系统盘输入 50GB,输入盘按照输入来选择； 获取对应价格（区域、架构、CPU、内存、磁盘等）。
   b. 购买时长固定为1年，计费方式为包年包月。
   c. 获取单台主机的预估费用。
   d. 用单台费用乘以数量，得到该类型总费用。
2. 累加所有类型的费用，输出总价。
3. 输出每种类型的详细配置、单价、数量、总价，以及所有类型的总费用。
4. 结果说明：价格为阿里云官网实时查询，仅供参考。

Outputs:
  - details: 每种类型的配置、单价、数量、总价明细、所选ECS实例规格（如ecs.g7.4xlarge等）
  - total: 总费用
  - disclaimer: "价格为阿里云官网实时查询，仅供参考，实际以官网为准。各型号规格以阿里云ECS实例规格族为估算依据，详情见details字段。"

Example Input:
region: "华北（北京）"
architecture: "x86"
configs:
  - cpu: 4
    memory: 16
    count: 3
  - cpu: 8
    memory: 32
    data_disks:
      - size: 100
        count: 2
    count: 5

Example Output:
details:
  - config: {cpu: 4, memory: 16, system_disk: 50}
    ecs_instance_type: ecs.g7.xlarge
    unit_price: 3500
    count: 3
    subtotal: 10500
  - config: {cpu: 8, memory: 32, system_disk: 50, data_disks: [{size: 100, count: 2}]}
    ecs_instance_type: ecs.g7.2xlarge
    unit_price: 7000
    count: 5
    subtotal: 35000
total: 45500
disclaimer: "价格为阿里云官网实时查询，仅供参考，实际以官网为准。各型号规格以阿里云ECS实例规格族为估算依据，详情见details字段。"
