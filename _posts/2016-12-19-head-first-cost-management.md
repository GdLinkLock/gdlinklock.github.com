---
layout: page
title: "成本管理-注意盈亏"
comments: true
tags: [HeadFirstPMP 项目管理]
categories: Books 
header:
   image_fullwidth: "header_unsplash_library.jpg"
---
# 成本管理过程

> 每个项目说到底都是钱的问题

## 成本估算过程(Cost Estimating Process)

* 输入：项目管理计划、项目范围说明书、组织过程资产、企业环境因素、工作分解结构、WBS词典
* 输出
    * 活动成本估算值
        * 活动清单中所有活动的成本估算，它考虑了资源的费率与活动的预计估期
    * 活动成本估算辅助细节
        * 类似于WBS有WBS词典
    * 成本管理计划更新
    * 请求的变更
* 工具方法
    * 确定资源成本率(Determine Resource Cost Rates)
        * 执行项目的每个人都有其费率，建造项目所使用的材料也有一定价格；这表示算出人工与材料的费率为多少
    * 供应商投标分析(Vendor Bid Analysis)
    * 储备分析(Reserve Analysis)
        * 留出一些钱以防止成本超出预算
    * 质量成本(Cost of Quality)
        * 要将项目做正确所必须花的钱
* 与预算相关的数字
    * 效益成本比率(Benefit Cost Ratio,BCR)
        * 项目能赚到的钱与项目成本的比值，越高越好
    * 净现值(Net Present Value,NPV)
        * 在给定时间中扣除所有相关成本后项目的实际价值
    * 机会成本(Opportunity Cost)
        * 当组织必须在两个项目间做选择时，总是得放弃他们可以赚但没有赚到的钱，这叫机会成本。它是因为你选择没做某个项目而没有得到的钱
    * 内部收益率(Internal Rate of Return)
        * 这是项目会回馈给出资公司的钱，通常会表示成投入资金的百分比
    * 折旧(Depreciation)
        * 这是项目随时间折损价值的速度。因此，如果你正在建造的项目只会在短时间内高价出售，则其产品便随着时间明显地折损价值
    * 生命周期成本(Lifecycle Costing)
        * 在项目开始之前，算出你预料它会花费多少

## 成本预算过程(Cost Budgeting Process)

* 输入：活动成本估算值，活动成本估算辅助细节、项目管理计划、项目范围说明书、组织过程资产、企业环境因素、工作分解结构、WBS词典
* 输出
    * 成本基线
    * 项目资金需求
* 如何建立成本预算
    * 累加估算值到控制账目
        * 又叫成本汇总(cost aggregation),将你的活动估算值累加近WBS的control account，这让你很容易知道项目中每一个 work package将花费多少
    * 算出储备金
        * 评估项目风险时，预留一些储备金来处理任何可能遇到的问题
    * 建立基线
        * 将实际情况与基线做比较，可以知道执行状况与基线之间的差异
    * 使用参数估算
    * 确认没有超出你的预算
        * 这项工具是资金限制平衡，Funding Limit  Reconciliation
    * 算出资金需求(Funding Requirement)
        * 必须计划何时以及如何使用
    * 更新成本计划

## 成本控制过程(Cost Control Process)

* 工具与技术
    * 成本变更控制系统(Cost Change Control System)
        * 定义了变更成本基线要遵循的步骤
    * 项目管理软件(Project Management Software)
    * 绩效测量分析(Performance Measurement　Analysis)
        * 测量项目执行现况并与计划做比较
    * 偏差分析(Variance Analysis)
    * 项目绩效评审(Project Performance Review)
    * 预测(Forecasting)
* 完工预算(Budget at Completion,BAC)
    * 项目总预算，将每个活动与资源的成本加总
* 如何计算PV(计划价值)
    * 如果你查看进度表并看到你应该已经完成所有工作的某个百分比，那就是你目前已经获取的总预算百分比。这个值称为“计划价值”(Planned Value,PV)
    * PV=BAC*Planned%Complete
* 挣值(Earned Value,EV)
    * 衡量你已经交付给顾客多少项目价值
    * 计算方式：EV=BAC*Actual%Complete
* 项目进度超前还是落后？
    * 进度绩效指数(Schedule Performance Index,SPI)
        * SPI=EV/PV
        * 当SPI大于1说明超越进度，当SPI小于1说明落后进度
    * 进度偏差(Schedule Variance,SV)
        * SV=EV-PV
        * 若偏差为正表示领先了多少钱，若偏差为负表示落后了多少钱
* 超出预算了吗？   
实际成本Actual Cost (AC)
    * 成本绩效指数(Cost Performance Index,CPI)
        * CPI=EV/AC
    * 成本偏差(Cost Variance)
        * CV=EV-AC
* 完工估算(Estimate at Completion,EAC)
    * 预估项目结束时总成本
    * EAC=BAC/CPI
* 完工尚需估算(Estimate to Completion)
    * 衡量可能还需要花多少钱在项目上
    * ETC=EAC-AC
* 完工偏差(Variance at Completion,VAC)
    * 预估项目结束时会有多少偏差
    * VAC=BAC-EAC