# FullStack-special-topic-modules

基於 **ASP.NET Core MVC** 開發，整合即時聊天室（SignalR）、通知中心與 FAQ 管理模組。  

---

## 目錄
- [簡介](#簡介)
- [功能亮點](#功能亮點)
- [系統模組介紹](#系統模組介紹)
  - [客服中心 (Customer Service Center)](#1-客服中心customerservice-center)
    - [客服評價管理子模組 (Customer Support Feedback)](#客服評價管理子模組customer-support-feedback)
  - [通知中心 (Notification Center)](#2-通知中心notification-center)
  - [幫助中心 (Help Center / FAQ)](#3-幫助中心help-center--faq)
- [資料庫設計](#資料庫設計)
- [系統架構](#系統架構)
- [技術亮點](#技術亮點)
- [作品示範影片](#作品示範影片)

---

## 簡介
本系統提供：
- 客服工單完整流程（建立、指派、處理、關閉）
- 即時雙向通訊（SignalR）與附件上傳
- 自動工單分派（負載最少優先）
- 通知中心（未讀/已讀、即時推播）
- FAQ 管理與前台即時搜尋

---

## 功能亮點
- 工單狀態即時推播，前端同步更新
- 完整工單編號規則：CSTyyMMddxxxx
- 三層式架構：Controller → Service → Repository
- 前後端分離的評價管理（AJAX / JSON API）
- 具角色權限（SuperAdmin / CustomerService / Customer）

---

## 系統模組介紹

### 1. 客服中心（Customer Service Center）
- 工單建立、指派、狀態追蹤（待處理 / 處理中 / 已完成）
- 智慧工單分派：自動指派給目前待處理工單數量最少的客服人員
- 角色權限顯示（SuperAdmin / CustomerService）
- 即時客服聊天室（SignalR）：文字與附件
- 工單狀態變更即時推播至前端
- 工單完成後觸發通知與後續流程

#### 客服評價管理子模組（Customer Support Feedback）
    - 完成工單後顯示評價表單（僅非「待處理」狀態可評價）
    - 後台限制：客服與管理員可存取（EmployeeAuth + Policy）
    - 提供評價清單、詳細查詢與刪除
    - 前後端分離：API + AJAX
    - 商業邏輯集中於 Service 層

---

### 2. 通知中心（Notification Center）
- 事件驅動式通知（如客服回覆、工單完成）
- 根據角色（Customer / Employee）推播
- 未讀 / 已讀 管理（支援單筆或全部標示已讀）
- 提供 API 供前端 Layout 顯示未讀數與最新通知
- 模組化設計，可被多模組共用

---

### 3. 幫助中心（Help Center / FAQ）
- FAQ 與分類管理（CRUD）
- 商業規則：若分類下仍有 FAQ，不允許刪除分類
- 後台管理介面 + RESTful API
- 前台支援 AJAX 即時查詢與分類顯示
- 使用 EF Core 與分層架構（提高可維護性）

---

### 資料庫設計
- CustomerSupportTickets
  - TicketID (PK), CustomerID (FK), EmployeeID (FK), Subject, TicketTypeID, StatusID, PriorityID, CreateTime, UpdateTime, TicketCode
- CustomerSupportMessages
  - MessageID (PK), TicketID (FK), SenderID, ReceiverID, MessageContent, SentTime, AttachmentURL
- CustomerSupportFeedback
  - FeedbackID (PK), TicketID (FK), CustomerID, FeedbackRating, FeedbackComment, CreateTime
- FAQs
  - FAQID (PK), Question, Answer, CategoryID (FK), IsActive, IsHot, HotOrder
- FAQCategorys
  - CategoryID (PK), CategoryName
- Notifications
  - NotificationID (PK), CustomerID (FK), Title, Message, Type, IsRead, CreatedAt, ReadAt
- Lookup tables
  - TicketTypes (TicketTypeID, TicketTypeName)
  - TicketStatus (StatusID, StatusDesc)
  - TicketPriority (PriorityID, PriorityDesc)

關係簡述：
- Tickets 1 — 多 Messages/Feedback
- TicketType/Status/Priority 為 Tickets 的查表
- FAQCategorys 1 — 多 FAQs
- Customers 1 — 多 Notifications


---

## 系統架構
三層式架構：Controller → Service → Repository → EF Core / SQL Server

- 每個模組皆採用分層設計，確保可測試、易維護與可擴充。

---

## 技術亮點
- 前端：AJAX, jQuery, SignalR
- 後端：ASP.NET Core MVC（Controller/Service/Repository）
- 資料庫：SQL Server, EF Core
- 其他：Git 版本控制、Session 驗證

---

## 作品示範影片
https://youtu.be/L24zNp1kEuU

- 客服工單建立、評價功能
- 即時聊天室與通知推播
- FAQ與分類展示
---