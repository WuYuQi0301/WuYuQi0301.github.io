---
title: Software Analysis Design HW9
date: 2019-05-06 23:30:00
categories:
- System Analysis & Design
tag:
- homework

---



System Analysis Course homework 6 on topic *Domain Modeling-Object State*.



使用 **UMLet** 建模

- 1、**使用类图，分别对 Asg_RH 文档中 Make Reservation 用例以及 Payment 用例开展领域建模。然后，根据上述模型，给出建议的数据表以及主要字段，特别是主键和外键**

  - make reservation

    ![pay](https://ww2.yunjiexi.club/2019/06/04/HlU2C.jpg)

    - 数据建模

      > Customer(ID/key, FullName, EmailAdress)
      >
      > Location(ID/key, Name)
      >
      > Hotel((ID/Key, name, address, introduction, roomIDList)
      >
      > Room(ID/Key, type, availability, price)
      >
      > RoomDescription(RoomID/FKey, type, total, price, description)
      >
      > ReservationItem(ID/key, RoomID/Fkey, BasketID/Fkey, checkInDate, checkOutDate, NumberOfAdults, NumberOfChildren, Cost)
      >
      > ReservationBasket(ID/key, CustomerID/Key)

  - payment

    ![mr](https://ww1.yunjiexi.club/2019/06/04/HlQxq.jpg)

    

    - 数据建模

      > Payment(ID/key, Cost, reservationID/Fkey, cardID/Fkey)
      > ReservationItem(ID/key, paymentID/FkeyID, RoomID/Fkey, BasketID/Fkey, checkInDate, checkOutDate, NumberOfAdults, NumberOfChildren, Cost)
      > CreditCard(ID/key, Type, CardSecurityCode, ExpiryDate, CustomerID/Fkey)
- 2、使用 UML State Model，对每个订单对象生命周期建模
  - 建模对象： 参考 Asg_RH 文档， 对 Reservation/Order 对象建模。
  - 建模要求： 参考练习不能提供足够信息帮助你对订单对象建模，请参考现在 定旅馆 的旅游网站，尽可能分析围绕订单发生的各种情况，直到订单通过销售事件（柜台销售）结束订单。

