---
title: Scrum 敏捷開發
date: 2019-10-29 18:05:47
tags:
---

# Scrum 是什麼？
Scrum 是一個流程框架，從 1990 年代初期由 Ken Schwaber 和 Jeff Sutherland 共同發展出來的，他被用在研究和辨識市場、技術、產品性能的可能性，以及開發產品和加強功能，人們透過運用這個框架來處理錯綜複雜的調適性問題，並且講求團隊運用其生產力和創意來盡可能的交付高價值的產品。

"A framework within which people can address complex adaptive problems, while productively and creatively delivering products of the highest possible value."
-- [The Scrum Guide](https://scrumguides.org/index.html)

# Scrum 如何運作？
將所有工作事項切分成多個可量化執行時間、工作量的小任務，並依照優先順序分配小任務於每個開發週期中，這個週期稱為 sprint，也就是快速衝刺的意思，通常以 2-4 週為一個 sprint，團隊成員必須於時限內完成計畫目標，當一個 sprint 結束時，會將完成的任務整理成一個可檢視的成果，再繼續進行下一個週期，反覆循環直到專案完成。

在管理進程上，會此用 Scrum board 來視覺化表示進度，例如將 board 分為 story（使用情境）、todo（細節功能）、WIP(work-in-process)、to verify（QA、test） 和 done 這幾個欄位，並用一張便利貼表示一個任務，當任務有進度時，便將便利貼移動到所屬進度，依照狀態及時挪動，直到完成，這樣的作法可以讓團隊成員共享資訊並清楚所有工作流程和進度。

scrum 的運作依靠以下重要物件、活動、特質：

1. 使用者故事
2. task: 根據 user story 列出所有需要被完成的任務
3. product backlog: 產品代辦清單
4. sprint backlog: 衝刺代辦清單
5. product increment: 可隨時發佈的可運作產品
6. sprint meeting
7. daily srcum
8. sprint review meeting
9. sprint retrospective meeting
10. 團隊專注、勇氣、開放、承諾和尊重

# 專案角色

在 scrum 中需要有明確的角色分配，主要角色為：
1. product owner 代表客戶立場，整合客戶意見、使用者回饋，確保開發中產品符合需求，負責規劃使用者故事，並與團隊討論
決定開發優先順序，在開發過程協助團隊釐清產品需求。在每個 sprint 週期結束時，決定使否將此週期的成果發佈。

2. scrum master
確保專案按照 scrum 方式開發，並排除任何會影響 scrum 流程的障礙，是規則的執行者。

3. develop team
負責開發和交付產品，包括設計、工程等個領域的專業人士。

其他角色可能為：
stackholder 利益關係者，可能是客戶

# kanban v.s. scrum

kanban 也是一種敏捷開發框架，由豐田汽車的創辦人 豐田喜一郎開發出來的一套管理方法，以「減少浪費」、「持續改進」為生產管理目標，也使用 kanban board 來視覺化作業流程，限制半成品（work-in-progress）的數量，監控與管理作業流程，建立明確的規劃，建立良好的訊息回饋路徑，團隊間互相合作，帶著實驗精神展開持續的改善。屬於長期的工作計畫，可以在任何時間點，透過團隊討論達成共識，改變專案執行方式和流程，沒有明確的專案角色，視專案和團隊需求而定，同時持續發布產品更新，由團隊成員共同決定是否發佈。

兩者的主要不同之處為：
- scrum 限制每個任務執行時間，kanban 主要限制同時執行的任務數目
- scrum 有明確的專案角色，kanban 沒有
- scrum 有明確的週期時間，kanban 是長期持續進行
- scrum 在一個 sprint 結束後發佈產品，kanban 因為沒有明確週期，所以可以隨時發佈產品

兩者的相同之處是：
- 採用視覺化的流程管理，公開透明化讓團隊成員可以共同討論、檢視。
- 有週期性檢閱成果，持續改善優化工作內容與流程
- 將複雜龐大的工作項目切分成多項可執行小任務，提升工作效率。
