---
title: Software-Analysis-Design-HW1
date: 2019-03-13 10:08:29
categories:
- System Analysis & Design
tag:
- homework
---

System Analysis Course homework 1 on topic *The essence of software and software engineering science*.



## 简答题

- 软件工程的定义 

  1. the application of a systematic, disciplined, quantifiable approach to the development, operation, and maintenance of software, that is, the application of engineering to software;（一个系统化规范化可度量的方法在软件开发、操作和维护上的应用，也就是说，将工程学应用在软件上）
   2. the study of approaches as in (1) （对于1中所说的方法的研究）




- 解释导致 software crisis 本质原因、表现，述说克服软件危机的方法

  - **表现**：
    1. 软件开发成本难以控制
    2. 软件开发进度难以预测
    3. 用户对产品功能的要求难以满足
    4. 软件质量无法保证
    5. 软件维护困难

  -  **原因**：
     1. 用户需求不明确
     2. 缺乏正确的理论指导
     3. 软件开发法规模越来越大，复杂度与越来越高

  - **克服方法**：研究软件生产的客观规律性，建立与系统化软件生产有关的概念、原则、方法、技术和工具，指导和支持软件系统的生产活动，以期达到降低软件生产成本 、改进软件产品质量、提高软件生产率水平的目标。

    


- 软件生命周期

  > 问题 定义及规划，需求分析，软件设计，编码实现，软件测试，运行维护

  

- SWEBoK 的 15 个知识域（[An Overview of the SWEBOK Guide](https://www.sebokwiki.org/wiki/An_Overview_of_the_SWEBOK_Guide) 请中文翻译其名称与简短说明）

  - **软件工程实践知识域** Knowledge Areas Characterizing the Practice of Software Engineering 

  | 英文                                       | 中文               | 简短说明                                                     |
  | ------------------------------------------ | ------------------ | ------------------------------------------------------------ |
  | Software Requirements                      | 软件需求           | 需求的提出、协商、分析、规范和确认。                         |
  | Software Design                            | 软件设计           | 定义系统或组件的架构、组件、接口和其他特性的过程，以及该过程的结果； |
  | Software Construction                      | 软件构造           | 通过详细设计、编码、单元测试、集成测试、调试和验证的组合来详细创建工作软件 |
  | Software Testing                           | 软件测试           | 评估产品质量并通过识别缺陷来改进产品质量的活动，涉及根据有限的测试用例集上的预期行为对程序行为进行动态验证 |
  | Software Maintenance                       | 软件维护           | 包括增强现有的能力，使软件适应新的和修改过的操作环境，以及纠正缺陷 |
  | Software Configuration Management          | 软件配置管理       | 系统地控制配置的更改，包括硬件、固件、软件的功能和/或物理特性，或这些特性的组合，并在整个软件生命周期中保持配置的完整性和可追溯性 |
  | Software Engineering Management            | 软件工程管理       | 包括计划、协调、测量、报告和控制项目或程序，以确保软件的开发和维护是系统的、有纪律的和量化的 |
  | Software Engineering Process               | 软件工程过程       | 软件生命周期过程的定义、实现、评估、测量、管理和改进         |
  | Software Engineering Models and Methods    | 软件工程模型和方法 | 描述了包含多个生命周期阶段的方法                             |
  | Software Quality                           | 软件质量           | 包括软件质量基础（软件工程文化、软件质量特征、软件质量的价值和成本以及软件质量改进）；软件质量管理过程（软件质量保证、验证和确认、评审和审计）；以及实用性 |
  | Software Engineering Professional Practice | 软件工程职业实践   | 涉及软件工程师以专业、负责和道德的方式实践软件工程所必须具备的知识、技能和态度 |

  - **软件工程教育需求知识域**  Knowledge Areas Characterizing the Educational Requirements of Software Engineering 

    | 英文                           | 中文           | 简短说明                                                     |
    | ------------------------------ | -------------- | ------------------------------------------------------------ |
    | Software Engineering Economics | 软件工程经济学 | 关注于在业务环境中做出决策，以使技术决策与组织的业务目标保持一致 |
    | Computing Foundations          | 计算基础       | 包括问题解决技术、抽象、算法和复杂性、编程基础、并行和分布式计算基础、计算机组织、操作系统和网络通信 |
    | Mathematical Foundations       | 数学基础       | 包括集合、关系和函数；基本命题和谓词逻辑；证明技术；图和树；离散概率；语法和有限状态机；以及数论 |
    | Engineering Foundations        | 工程基础       | 包括经验方法和实验技术；统计分析；测量和度量；工程设计；模拟和建模；以及根本原因分析 |

- 简单解释 CMMI 的五个级别。例如：Level 1 - Initial：无序，自发生产模式。

  1．**初始级**：软件过程是无序的，有时甚至是混乱的，对过程几乎没有定义，成功取决于个人努力。管理是反应式的。

  2．**可管理级**：建立了基本的项目管理过程来跟踪费用、进度和功能特性。制定了必要的过程纪律，能重复早先类似应用项目取得的成功经验。

  3． **已定义级**：已将软件管理和工程两方面的过程文档化、标准化，并综合成该组织的标准软件过程。所有项目均使用经批准、剪裁的标准软件过程来开发和维护软件，软件产品的生产在整个软件过程是可见的。

  4． **量化管理级**：分析对软件过程和产品质量的详细度量数据，对软件过程和产品都有定量的理解与控制。管理有一个作出结论的客观依据，管理能够在定量的范围内预测性能。

  5． **优化管理级**：过程的量化反馈和先进的新思想、新技术促使过程持续不断改进。






- 用自己语言简述 SWEBok 或 CMMI 

  > **SWEBok**（SoftWare Engineering Body Of Knowledge）, 软件工程知识体系描述了软件工程学科的内容,促进了对全球软件工程的一致看法，明确了软件工程相对于其他专业的位置和界限，并为培训材料和课程开发提供基础，为软件工程师的认证和许可提供依据。Swebok v3包含15个知识领域（KAS），其中有软件需求、软件设计、软件构件、软件测试、软件维护等软件工程体系下的细分领域。