# Công cụ hỗ trợ chấm và giải thích bài kiểm tra tự luận – Thiết kế phần mềm

**CSC13114 – PA#1** · **Nhóm:** Trịnh Hồ Minh Thắng (`23120354`), Võ Hữu Thắng (`23120355`),Trần Vũ Quang (`23120356`) 

## 1. Người dùng và vấn đề

**Người dùng:** thầy Hưng, giảng viên môn Thiết kế phần mềm, lớp ~40 sinh viên. Sau mỗi bài kiểm tra giữa kỳ thầy chấm 5 câu tự luận ngắn (giải thích nguyên tắc thiết kế, chọn design pattern cho một tình huống), tức 200 câu.

**Vấn đề:** chấm tay 200 câu mất khoảng 6–7 giờ, và thầy vẫn không có thời gian viết giải thích điểm chi tiết cho từng sinh viên.

**Hiện giờ:** thầy chấm tay từng bài, ghi nhận xét ngắn, ~10 phút/bài.

**Sản phẩm:** web app. Thầy soạn câu hỏi, rubric, đáp án tham khảo; sinh viên làm bài trên web; LLM đề xuất điểm và giải thích; thầy duyệt xong sinh viên mới thấy kết quả.

## 2. Tính năng LLM và cái giá của việc sai

**Một tính năng:** với mỗi bài làm, LLM đề xuất điểm theo từng tiêu chí rubric, giải thích vì sao được hoặc mất điểm, **trích nguyên văn đoạn trong bài làm** làm căn cứ và nêu phần còn thiếu so với đáp án. Ví dụ một tiêu chí: `{điểm: 1/2, trích: "<nguyên văn>", thiếu: "chưa nêu Open/Closed"}`. Điểm LLM chỉ là đề xuất; thầy duyệt và sửa trước khi công bố.

| Lỗi | Ai bị hại, bao nhiêu, hoàn tác được không | Giảm bằng |
|---|---|---|
| Chấm sai điểm | Sinh viên bị chấm oan hoặc được điểm không xứng, ảnh hưởng điểm quá trình. Lỗi có tính hệ thống: một tiêu chí hiểu sai lặp lại trên cả 40 bài. Sửa dễ **trước** công bố; sau công bố phải phúc khảo, tốn công cả hai phía | Chỉ công bố sau khi thầy duyệt; rubric chia tiêu chí nhỏ; căn cứ hiện cạnh từng điểm |
| Giải thích sai hoặc bịa | Sinh viên hiểu sai kiến thức (nhầm pattern, SOLID) và mang sang kỳ thi sau; khó phát hiện vì sinh viên tin lời giải thích | Đoạn trích phải có trong bài làm (kiểm tra bằng code); thầy duyệt cả phần giải thích |
| Thầy duyệt qua loa vì tin LLM | Lỗi lọt qua đúng khâu chặn lỗi: "sai mà không ai biết" | Đo tỉ lệ thầy sửa điểm để phát hiện duyệt quá nhanh |

**Làm sao biết nó sai:** so điểm LLM với điểm thầy chấm tay trên cùng bộ bài. Ngưỡng: độ lệch tuyệt đối trung bình ≤ 0,7 điểm (thang 10); bài lệch hơn 1 điểm ≤ 10%; giải thích sai hoặc không có căn cứ ≤ 5%. Vượt ngưỡng thì chưa đủ tin để dùng. Giới hạn cứng: độ dài bài làm, số bài mỗi lần chấm.

**Dữ liệu gửi ra ngoài:** câu hỏi, rubric, tài liệu bài giảng và bài làm đã bỏ tên, MSSV gửi tới nhà cung cấp LLM; ở gói miễn phí họ có thể dùng nội dung này để cải thiện sản phẩm. Vì vậy khi thử nghiệm nhóm **chỉ dùng bài do thành viên và bạn bè tự viết** (giỏi, trung bình, yếu), không dùng bài sinh viên thật.

## 3. Phạm vi

**In (trong học kỳ):** soạn câu tự luận kèm rubric, đáp án, tài liệu bài giảng PDF có chữ hoặc DOCX (tùy chọn); sinh viên làm bài bằng văn bản; LLM đề xuất điểm và giải thích; giảng viên duyệt, sửa, công bố (PA#3); eval so với điểm giảng viên, guardrail, red-team (PA#4); triển khai, quan trắc (PA#5).

**Out (cố ý không làm):** câu trắc nghiệm; LLM tự tạo câu hỏi; chat với LLM về đáp án; phát hiện gian lận hoặc bài do AI viết; chấm sơ đồ UML dạng ảnh, chữ viết tay, ảnh scan; chấm code chạy được; phúc khảo trong hệ thống; quản lý lớp học, điểm danh, học phí, thời khóa biểu; đầu ra ngôn ngữ khác tiếng Việt.

## 4. Kế hoạch

Mỗi checkpoint một người phụ trách; hai người còn lại làm phần được giao và review PR. Ngày bám nhịp một checkpoint mỗi tuần của môn học.

| Checkpoint | Ngày | Phụ trách | Công việc → đầu ra kiểm tra được |
|---|---|---|---|
| PA#1 | 8/10/2026 | Trịnh Hồ Minh Thắng | Proposal, chốt phạm vi, tạo repo → proposal + repo + self-assessment |
| PA#2 | 15/10/2026 | Võ Hữu Thắng | Harness: file quy ước, lint, test, CI → PR lỗi lint/test bị CI chặn merge |
| PA#3 | 22/10/2026 | Trần Vũ Quang | Prototype tạo bài → làm bài → LLM đề xuất → duyệt → 1 bài mẫu chạy đủ luồng; đầu ra LLM kiểm tra bằng code; sinh viên chỉ thấy điểm sau khi duyệt |
| PA#4 | 29/10/2026 | Trịnh Hồ Minh Thắng | Eval, guardrail, red-team → eval 30 bài × 5 câu theo ngưỡng mục 2; biên bản red-team 10 tình huống |
| PA#5 | 5/11/2026 | Võ Hữu Thắng | Hoàn thiện, triển khai, quan trắc → bản chạy thật có log/đo lường; 1 biên bản hậu kiểm sự cố |
| PA#6 | 12/11/2026 | Trần Vũ Quang | Chuẩn bị vấn đáp → cả 3 người giải thích được toàn bộ code và quyết định của nhóm |

## 5. Rủi ro

**R1 – LLM chấm lệch nhiều so với giảng viên hoặc giải thích sai** (tính năng chính mất giá trị).
*Bắt đầu tuần này (8–14/10):* viết 1 câu tự luận + rubric; mỗi người viết 4 bài trả lời (giỏi/trung bình/yếu, tổng 12 bài); nhờ một giảng viên chấm tay; chạy LLM, tính độ lệch theo ngưỡng mục 2. Nếu vượt ngưỡng, đổi cách trước PA#3 (chia rubric nhỏ hơn, thêm ví dụ chấm mẫu, thử model khác).

**R2 – Hết hạn mức gói miễn phí hoặc model bị đổi/ngừng đúng lúc eval, demo** (chấm cả lớp là rất nhiều lần gọi).
*Bắt đầu tuần này:* đọc hạn mức thật trong console AI Studio; viết script đo token và số lần gọi, chạy thử 20 lần; eval ~150 lần gọi (~450K token), ở mức thấp 5 yêu cầu/phút mất ~30 phút nên khả thi; thêm hàng đợi và retry; đăng ký sẵn Groq; lưu bộ kết quả chấm để demo khi dịch vụ lỗi.

## 6. Công nghệ

- **LLM: Gemini 3.5 Flash-Lite qua Google AI Studio (dự phòng 3.1 Flash-Lite):** gói miễn phí không cần thẻ, có đầu ra có cấu trúc nên kiểm tra đoạn trích bằng code được. Hạn mức xem tại `aistudio.google.com/rate-limit`; nguồn bên ngoài nêu ~15 yêu cầu/phút, ~1.000 yêu cầu/ngày nên thiết kế theo mức thấp. Nếu phải trả phí, giá tham khảo $0,30 / $2,50 mỗi triệu token vào/ra (nguồn bên thứ ba): một lượt eval (~150 lần gọi, ~2.300 token vào, ~700 ra mỗi lần) khoảng 0,4 USD.
- **Groq (phương án hai: Mistral):** miễn phí không cần thẻ, tránh phụ thuộc một nhà cung cấp (R2); cùng lưu ý dữ liệu, chỉ dùng bài tự viết.
- **Hàng đợi + retry:** chấm cả lớp là hàng trăm lần gọi, cần để không vượt hạn mức.
- **React + Vite + TypeScript, Node.js + Express/Fastify (TypeScript):** cả nhóm đã học ở môn này; kiểu dữ liệu dùng chung giúp kiểm tra đầu ra LLM bằng code.
- **PostgreSQL (Docker):** lưu bài, rubric, điểm đề xuất và điểm đã duyệt; tách hai loại điểm để đo tỉ lệ thầy sửa.
- **pdf-parse + mammoth:** đọc tài liệu bài giảng PDF/DOCX và cắt đoạn để đối chiếu khi chấm.
- **Vitest, Playwright, GitHub Actions:** CI chặn merge khi lỗi; Playwright test luồng "giảng viên duyệt rồi sinh viên mới thấy điểm".
