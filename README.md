# POD Ad Performance — Hướng Dẫn Đọc Dashboard

Dashboard này giúp bạn xác định đúng vấn đề, và action nhanh hơn.

---

## Cấu trúc flow đọc dashboard

```mermaid
flowchart TD
    A([Bước 1 — KPI tổng quan\nROAS & CPP]) --> B{Cả hai đạt?}

    B -->|ROAS ≥ 1.5x & CPP ≤ $25| C([Đang tốt\nXem ad/SP nào đang kéo lên])
    B -->|Chưa đạt| D([Bước 2 — Phễu chuyển đổi\nTìm bước drop lớn nhất])

    C --> E([Bước 3 — Ma trận Ad / Sản phẩm])

    D --> F([Drop Reach → ATC\nATC Rate thấp])
    D --> G([Drop Checkout → Purchase\nTỷ lệ mua thấp])

    F --> F1[Review tiếp cận\nTargeting & creative]
    G --> G1[Review giá & sản phẩm]

    F1 --> E
    G1 --> E

    E --> E1{0 purchase?}

    E1 -->|Spend > $25| CUT1([Cut\nĐã chi đủ nhưng không ra đơn])
    E1 -->|ATC Rate ≥ 0.5%| SIG([ATC Signal\nCó tín hiệu — chờ thêm data])
    E1 -->|Spend ≤ $25| TEST([Testing\nChưa đủ data để kết luận])

    E1 -->|Có purchase| E2{ROAS?}

    E2 -->|ROAS < 1.0x| CUT2([Cut\nĐang lỗ])
    E2 -->|ROAS ≥ 1.5x & CPP ≤ $25| SCALE([Scale\nTăng budget])
    E2 -->|Còn lại| OPT([Optimize\nTest & cải thiện])

    SCALE --> K([Bước 4 — IMG vs VID\nSo sánh normalized theo spend])
    OPT --> K
    CUT1 --> L[Dừng ngay\nReinvest vào ad/SP đang Scale]
    CUT2 --> L

    K --> M([Bước 5 — Track mỗi ngày])

    style A fill:#EEEDFE,stroke:#534AB7,color:#26215C
    style B fill:#E6F1FB,stroke:#185FA5,color:#042C53
    style C fill:#E1F5EE,stroke:#0F6E56,color:#04342C
    style D fill:#FCEBEB,stroke:#A32D2D,color:#501313
    style E fill:#EEEDFE,stroke:#534AB7,color:#26215C
    style E1 fill:#E6F1FB,stroke:#185FA5,color:#042C53
    style E2 fill:#E6F1FB,stroke:#185FA5,color:#042C53
    style F fill:#FAEEDA,stroke:#854F0B,color:#412402
    style G fill:#FAEEDA,stroke:#854F0B,color:#412402
    style F1 fill:#F1EFE8,stroke:#5F5E5A,color:#2C2C2A
    style G1 fill:#F1EFE8,stroke:#5F5E5A,color:#2C2C2A
    style CUT1 fill:#FCEBEB,stroke:#A32D2D,color:#501313
    style CUT2 fill:#FCEBEB,stroke:#A32D2D,color:#501313
    style SIG fill:#FAEEDA,stroke:#854F0B,color:#412402
    style TEST fill:#F1EFE8,stroke:#5F5E5A,color:#2C2C2A
    style SCALE fill:#E1F5EE,stroke:#0F6E56,color:#04342C
    style OPT fill:#FAEEDA,stroke:#854F0B,color:#412402
    style K fill:#EEEDFE,stroke:#534AB7,color:#26215C
    style L fill:#FCEBEB,stroke:#A32D2D,color:#501313
    style M fill:#E1F5EE,stroke:#0F6E56,color:#04342C
```

---

## Bước 1 — KPI tổng quan

**Câu hỏi:** Chiến dịch đang ở đâu so với kỳ vọng?

| KPI | Mục tiêu |
|---|---|
| ROAS | ≥ 1.5x |
| Cost Per Purchase (CPP) | ≤ $25 |

- Cả hai đạt → đi thẳng Bước 3 để xem ad/SP nào đang kéo lên.
- Một trong hai chưa đạt → đi tiếp Bước 2.

---

## Bước 2 — Phễu chuyển đổi

**Câu hỏi:** Budget đang bị mất ở bước nào?

```
Impressions → Reach → ATC → Checkout → Purchase
```

**Nếu drop ở Reach → ATC (ATC Rate thấp):**
Review phần tiếp cận — targeting và creative có đang đến đúng người không.

**Nếu drop ở Checkout → Purchase:**
Review giá và sản phẩm — khách đã quan tâm nhưng không mua.

Trong POD, khách bỏ ở bước này thường vì hai lý do chính:

- **Giá ship cao hoặc delivery time dài** xuất hiện muộn ở bước checkout — khách bị bất ngờ và bỏ đơn. Để cạnh tranh được, cần chọn fulfillment partner có giá sản xuất thấp và giá ship thấp vì đây ảnh hưởng trực tiếp đến giá bán cuối và khả năng cạnh tranh trên sàn.
- **Thiếu trust** tại thời điểm ra quyết định thanh toán — ít review, shop mới, không có social proof.

> ATC Rate = ATC ÷ Reach

---

## Bước 3 — Ma trận Ad / Sản phẩm

**Câu hỏi:** Ad nào / SP nào đang ở trạng thái nào?

Xem từng ad và từng sản phẩm theo logic sau:

```
Nếu 0 purchase:
  ├── Spend > $25              → Cut        (đã chi đủ nhưng không ra đơn)
  ├── ATC Rate ≥ 0.5%          → ATC Signal (có tín hiệu, chờ thêm data)
  └── Spend ≤ $25              → Testing    (chưa đủ data để kết luận)

Nếu có purchase:
  ├── ROAS < 1.0x              → Cut        (đang lỗ)
  ├── ROAS ≥ 1.5x & CPP ≤ $25 → Scale      (tăng budget)
  └── Còn lại                  → Optimize   (test & cải thiện)
```

| Nhãn | Hành động |
|---|---|
| Cut | Dừng ngay, reinvest vào ad/SP đang Scale |
| ATC Signal | Giữ nguyên, theo dõi thêm 1–2 ngày |
| Testing | Giữ budget thấp, chờ đủ data |
| Scale | Tăng budget, giữ nguyên những gì đang chạy |
| Optimize | Test creative mới, review giá |

> **Note:** Đây là framework mô tả chung. Mỗi sản phẩm và chiến dịch sẽ có ngưỡng định nghĩa khác nhau — ví dụ sản phẩm giá cao có thể cần spend > $50 mới đủ data, hoặc ATC Rate threshold có thể khác tùy niche.

**Lưu ý khi Scale trong POD:**
Khi volume đơn tăng nhanh — đặc biệt dịp lễ lớn (Christmas, Valentine, Mother's Day) — fulfillment partner phải đáp ứng được cả ba yếu tố:

- **Giá sản xuất & ship thấp** để giữ margin khi scale
- **Quy trình QC chặt chẽ** để giảm thiểu lỗi sản phẩm khi volume lớn
- **Đơn vị vận chuyển nghiêm túc, đúng SLA** — dịp lễ ship chậm vài ngày có thể dẫn đến refund hàng loạt, ảnh hưởng trực tiếp đến ROAS thực tế

---

## Bước 4 — IMG vs VID

**Câu hỏi:** Format nào đang hiệu quả hơn?

So sánh theo spend — không so raw volume.

- CPM của IMG thấp hơn → dùng IMG để test sản phẩm mới.
- CVR của VID cao hơn → khi sản phẩm đã có tín hiệu tốt, chuyển sang VID.
- Chênh lệch nhỏ → chưa kết luận, cần xem thêm theo niche hoặc spend tier.

---

## Bước 5 — Track mỗi ngày và action

Không đợi cuối tuần hay cuối tháng mới review.

- Ad/SP nào đạt → giữ và scale dần.
- Ad/SP nào không đạt → tắt ngay, không để chạy thêm.
- Reinvest ngân sách vừa giải phóng vào những gì đang hoạt động.

**Ngoài ads metrics, track song song:**

| Metric | Lý do cần theo dõi |
|---|---|
| Complaint rate | Sản phẩm lỗi, in sai, giao thiếu → ảnh hưởng review và trust |
| Refund rate | Ship chậm dịp lễ hoặc sản phẩm không đúng mô tả → kéo revenue thực xuống dù ROAS trông ổn |
| Fulfillment SLA | Đơn vị sản xuất có đúng deadline không — đặc biệt quan trọng khi đang scale mạnh |

> ROAS cao nhưng refund nhiều thì revenue thực tế vẫn thấp. Ads tốt chỉ có giá trị khi operations phía sau đủ vững để giữ được đơn hàng đó.

---

## Công thức tính nhanh

| Chỉ số | Công thức |
|---|---|
| ROAS | Revenue ÷ Spend |
| CPP | Spend ÷ Purchases |
| ASP | Revenue ÷ Purchases |
| ATC Rate | ATC ÷ Reach |
