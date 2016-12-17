---
layout: page
title: "时间管理-准时完成"
comments: true
tags: [HeadFirstPMP 项目管理]
categories: Books 
header:
   image_fullwidth: "header_unsplash_library.jpg"
---

# 时间管理过程

## 活动定义

* 输入：组织过程资产、企业环境因素、项目管理计划书、WBS、WBS词典、项目范围说明书
* 输出
    * 活动清单(Activity List)
        * 完成项目要做的所有事情的清单，此清单的分解活动比WBS层次还要低
    * 活动属性(Activity Attribute)
        * 记录每一项活动的描述，包含任何前导活动(predecessor activity)，后续活动(successor activity)，约束(constraint)
    * 里程碑清单(Milestone List)
        * 项目所有的重要检查点被记录为里程碑，里程碑清单必须让每个人知道什么是重要的，什么不是
    * 请求的变更(Requested Changes)
        * 随着你想出那些活动需要被完成，可能发现项目的范围需要变更
* 使用的方法
    * 分解(Decomposition)
        * 将范围管理诸多过程中所定义的work package 进一步分解为能被估算的activity
    * 模板(Template)
        * 来自已经做过的类似项目
    * 专家判断(Expert Judgment)
    * 计划组件(Planning Somponent)    
    当对项目了解不够，无法想出完整的活动清单时，可以使用计划组件作为占位符，直到掌握更多的信息；以下是两个组件：
        * 控制账目(Control Accout)
        * 计划包(Planning Package)
* 滚动式计划(Rolling Wave Planning)
    * 有时候我们对于稍后要做的工作所知有限，RWP使得我们就有限信息制定计划于进度

## 活动排序

* 输入：活动清单、活动属性、里程碑清单、变更请求
* 输出
    * 网络图：PDM or ADM
    * 活动属性更新
    * 活动清单更新
    * 变更请求
* 绘制activity之间的关系
    * 前导图法(Precedence Diagramming Method,PDM)
    *　箭线图法(Arrow Diagramming Method,ADM)
* 先后依存关系类型
    * 完成-开始：Finish to Start,FS
    * 开始-开始：Start to Start，SS
    * 完成-完成：Finish to Finish，FF

## 活动资源估算

资源是人、设备、场所或者完成全部活动所需的任何其他东西，活动清单中的每一项活动都需要指派资源

* 输入：活动清单、活动属性、资源可用性，企业环境因素，组织过程资产，项目管理计划；其中资源可用性是指那些资源可用以及何时可用的信息
* 输出：资源需求
* 资源估算
    * 专家判断(Expert Judgment)
    * 替代方案分析(Alternative Analysis)
    * 已发布的估算数据(Published Estimating Data)
    * 项目管理软件(Project Management Software)
    * 自下而上估算(Bottom-Up Estimating)

## 活动工期估算

* 输入：资源需求、活动清单、活动属性、资源日历、项目范围说明书、组织过程资产、企业环境因素
* 输出
    * 活动工期估算值(Activity Duration Estimating)：活动清单中每一项活动花多少时间的估算值
    * 活动属性：属性可能会发生变更
* 估算工具与技术
    * 专家判断(Expert Judgment)
    * 类比估算(Analogous Estimating)
        * 检查过去类似的项目活动，看看以前进行类似工作所花费时间
    * 参数估算(Parametric Estimating)
        * 将项目数据填入计算估算值的公式、电子表格、数据库或计算机程序
    * 三点估算(Three-Point Estimating)
        * 找出三个数字：现实的估算值是最可能发生的，乐观的估算值是最好情况下发生的，悲观的估算值是最坏情况下发生的数值；最后加权平均而得
    * 储备分析(Reserve Analysis)
        * 为进度增加额外时间，将额外“风险”考虑进来

## 进度制定

进度制定过程是时间管理的核心。要把目前所完成的每一件事组合成最后的进度表

* 输入：资源日历、网络图、活动清单、活动属性、活动资源需求、活动工期估算、组织过程资产、项目管理计划、范围说明书
* 输出    
    * 进度表
    * 里程碑清单
* 关键路径法(Critical Path Method)   
关键路径法是让项目保持在轨道上的重要工具。关键路径是一连串活动，将这些活动的所有工期加总后会超过穿过网络图中的任何其他路径
    * 找关键路径方法
        * 从网络图开始
        * 找出网络图中所有路径，一条路径就是一连串活动，从项目开始到项目结束
        * 对路径上所有工期求和就可以得到每条路径的工期
        * 关键路径是具有最长工期的路径
* 浮动时间  
任何活动的浮动时间是在“它造成项目滞后以前可以耽搁的时间量”。关键路径浮动时间为0
* 赶工(crash the schedule)
    * 表示增加或调用资源来缩短工期，通常表示花费更多以及并非总是有效
* 快速跟进项目(fast-tracking the project)
    * 计划中两个活动相继发生，但实际上可以同时进行
* 假设分析
    * 蒙克卡罗分析(Monte Carlo Analysis)
        * 使用计算机为不确定性建模
    * 进度模型(Schedule Model)
        * 使用项目管理软件创建进度模型并调整不同元素，看看可能会发生什么
* 其他进度网络分析技术
    * 关键链法(Critical Chain Method)
    * 资源平衡(Resource Leveling)
    * 运用日程表(Applying Calendars)
    * 调整提前与滞后(Ajusting leads and lags)

## 进度控制 

项目经理不只是等等变更的发生，他还需要找出会造成变更的事情并且影响它们

* 输入：范围管理计划、进度基线、绩效报告、批准的变更请求
* 输出：请求的变更、基线更新、建议的纠正措施、绩效测量
* 工具与技术
    * 偏差分析(Variance Analysis)
    * 进展报告(Progress Reporting)
    * 绩效测量(Performance Measurement)
    * 项目管理软件(Project Management Software)
    * 进度比较条形图(Schedule Comprasion Bar Charts)
    * 进度变更控制系统(Schedule Change Control System)