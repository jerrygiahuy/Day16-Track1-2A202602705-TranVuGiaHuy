# Memo Teardown — NotebookLM (nay là Gemini Notebook)

**Họ tên:** Trần Vũ Gia Huy

**Mã sinh viên:** 2A202602705

**Ngày phân tích:** 03/10/2026

**Vì sao chọn sản phẩm này:** NotebookLM là một sản phẩm AI-native: giá trị chính không nằm ở lưu ghi chú mà ở việc biến một tập nguồn do người dùng kiểm soát thành trợ lý nghiên cứu có trích dẫn. Sản phẩm có chuỗi quyết định công khai đủ dài để quan sát cách Google đi từ một thử nghiệm RAG hẹp sang một workspace nghiên cứu đa phương thức và có tính agent.

## §1. Timeline các cập nhật lớn

| Thời điểm | Cập nhật | Context lúc đó | Nguyên lý |
|---|---|---|---|
| 07/2023 | Google Labs đổi Project Tailwind thành [NotebookLM và mở thử nghiệm cho một nhóm nhỏ tại Mỹ](https://blog.google/innovation-and-ai/technology/ai/notebooklm-google-ai/). Sản phẩm trả lời trên tài liệu người dùng tải lên và gắn citation về đoạn gốc. | Sau làn sóng chatbot đa dụng cuối 2022–đầu 2023, vấn đề nổi bật là hallucination và câu trả lời khó kiểm chứng. NotebookLM chọn một phạm vi hẹp: “nguồn của tôi”, thay vì cố biết mọi thứ. | **Định nghĩa “tốt” bằng groundedness:** tối ưu câu trả lời có thể lần về nguồn, không chỉ tối ưu độ trôi chảy. Đây cũng là **wedge hẹp** để tạo trải nghiệm x10 so với Ctrl+F và đọc thủ công. |
| 12/2023 | [Mở rộng tại Mỹ, dùng Gemini Pro, thêm noteboard, suggested actions và xuất sang Docs](https://blog.google/innovation-and-ai/products/notebooklm-new-features-availability/). | Phản hồi từ knowledge workers, creators, sinh viên và giáo viên cho thấy họ không chỉ hỏi đáp mà cần đi từ đọc → ghi chú → viết. | **Workflow moat:** đặt AI vào cả chuỗi công việc, lưu lại trích dẫn, ghi chú và sản phẩm trung gian; giá trị vượt khỏi một “chat wrapper”. |
| 06/2024 | [Ra mắt tại hơn 200 quốc gia/vùng lãnh thổ trên Gemini 1.5 Pro](https://blog.google/intl/id-id/company-news/technology/google-meluncurkan-notebooklm-asisten-untuk-riset-dan-penulisan-berbasis-ai-di-indonesia/), hỗ trợ hiểu hình, biểu đồ và sơ đồ trong Docs/Slides. | Gemini 1.5 mở rộng context và năng lực multimodal; bản đầu bị giới hạn theo địa lý và loại nguồn. | **Model capability → product surface:** chuyển bước nhảy context/multimodal của model thành lợi ích cụ thể trên nguồn thật; đồng thời mở rộng phân phối toàn cầu. |
| 09/2024 | Ra mắt [Audio Overviews](https://blog.google/innovation-and-ai/products/notebooklm-audio-overviews/): hai host AI thảo luận, kết nối ý và tóm tắt nguồn thành audio tải về được. | Chat với tài liệu đã dần thành commodity. Nhiều người muốn học khi di chuyển hoặc ghi nhớ tốt hơn qua nghe. | **x10 qua đổi modality:** không chỉ làm đọc nhanh hơn mà biến tài liệu thành một trải nghiệm tiêu thụ mới. Output giàu cảm xúc tạo “magic moment” khó có ở công cụ search-in-PDF. |
| 12/2024 | [Thiết kế lại thành ba panel Sources–Chat–Studio, cho phép tham gia hội thoại audio và ra NotebookLM Plus](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-new-features-december-2024/) với hạn mức cao hơn, team notebook, analytics, privacy/security cấp doanh nghiệp. | Google công bố hàng triệu người và hàng chục nghìn tổ chức đã dùng sản phẩm; nhu cầu team/enterprise khác rõ nhu cầu cá nhân. | **Product-led growth → monetization:** giữ free tier tạo thói quen, thu tiền từ cường độ sử dụng, cộng tác và quản trị; **moat tổ chức** đến từ shared corpus, quyền truy cập và workflow chung. |
| 04/2025 | [Discover Sources](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-discover-sources/) tự quét web, chọn tối đa 10 nguồn phù hợp và nhập vào notebook. | Trước đó người dùng phải có sẵn nguồn và tự thu thập; đây là ma sát lớn ở đầu funnel nghiên cứu. | **Mở rộng job boundary:** đi ngược từ “hiểu nguồn đã có” sang “tìm nguồn nên đọc”. Vòng lặp **discover → curate → synthesize** giữ người dùng trong sản phẩm lâu hơn. |
| 07/2025 | [Video Overviews và Studio mới](https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-video-overviews-studio-upgrades/) tạo narrated slides, nhiều output cùng loại, mind map và report. | Audio Overview đã chứng minh nhu cầu biến cùng một corpus thành cách học phù hợp từng ngữ cảnh; nhưng audio không diễn đạt tốt số liệu, sơ đồ và quy trình. | **Một corpus, nhiều personalized artifacts:** tái sử dụng context người dùng đã đầu tư để tạo output theo modality, vai trò và trình độ — tăng learning loop và switching cost dữ liệu. |
| 06–07/2026 | Google nâng cấp NotebookLM với [agentic research, code execution và nhiều định dạng artifact](https://blog.google/innovation-and-ai/products/notebooklm/better-research-notebooklm/), sau đó [đổi tên thành Gemini Notebook](https://blog.google/innovation-and-ai/products/gemini-notebook/notebooklm-gemini-notebook/) và bắt đầu đồng bộ với Gemini/Search. | Cuộc cạnh tranh trợ lý nghiên cứu chuyển từ “tóm tắt tốt” sang tự lập kế hoạch, dùng công cụ và tạo deliverable. Các surface Gemini, Search và NotebookLM có nguy cơ chồng lấn. | **Từ copilot sang agent + distribution moat:** sản phẩm tiến từ hiểu corpus sang hành động trên corpus; Google tận dụng hệ sinh thái thay vì để NotebookLM là một island độc lập. |

**Vì sao chọn những mốc này:** Tôi chọn các mốc làm thay đổi một trong bốn thứ: job cốt lõi, modality, segment/mô hình thu tiền hoặc vị trí của sản phẩm trong hệ sinh thái. Tôi loại mốc hỗ trợ YouTube/audio source ngày 26/09/2024 vì chủ yếu mở rộng input sau bước ngoặt Audio Overview; cũng loại các lần thêm style/voice nhỏ vì không đổi logic sản phẩm.

## §2. Tệp user & JTBD

| | Early adopters (2023–đầu 2024) | Tệp hiện tại (2025–2026) |
|---|---|---|
| **Đặc điểm** | Nghiên cứu sinh, nhà văn/creator và knowledge worker tại Mỹ; đã có corpus Google Docs/PDF lớn, quen thử Google Labs và chấp nhận sản phẩm experimental. Một người điển hình là nghiên cứu sinh phải đối chiếu 20–50 paper cho literature review. | Sinh viên đại học, giáo viên, analyst, marketer, chủ doanh nghiệp nhỏ và team enterprise trên nhiều quốc gia; dùng cả web/mobile, cần học hoặc tạo deliverable từ PDF, video, audio, web và dữ liệu. Một người điển hình là analyst trong team phải biến bộ tài liệu dự án thành briefing có citation, bảng tính và deck. |
| **JTBD chính** | Khi có nhiều tài liệu riêng, giúp tôi tìm bằng chứng, nối ý và tạo first draft mà không phải đọc lại toàn bộ, để tôi tư duy nhanh hơn nhưng vẫn kiểm tra được nguồn. | Khi bắt đầu một chủ đề hoặc dự án phức tạp, giúp tôi tìm–lọc–hiểu nguồn và chuyển chúng thành định dạng phù hợp với người nhận, để tôi đi từ câu hỏi mơ hồ tới deliverable có căn cứ trong một workspace. |
| **Trước đó họ làm bằng cách nào** | Ctrl+F từng PDF/Doc, ghi chú bằng Docs/Notion/Zotero, copy đoạn quan trọng sang chatbot và tự quay lại kiểm tra trích dẫn. | Chuyển qua lại giữa Google Search/Scholar, chatbot Deep Research, Drive, công cụ podcast/video, Docs/Sheets/Slides và công cụ cộng tác của tổ chức. |
| **Cột mốc gây dịch chuyển** | Launch 07/2023 và workflow đọc–ghi–viết 12/2023 tạo wedge cho power users có sẵn nguồn. | Audio 09/2024 mở rộng sang người học đại chúng; Plus 12/2024 mở enterprise; Discover Sources 04/2025 giảm yêu cầu “phải có corpus”; mobile/video 2025 và agentic artifacts + Gemini integration 2026 mở rộng sang công việc đầu-cuối. |

**Dịch chuyển tệp:** Mốc 09/2024 là cú mở rộng lớn nhất ở consumer: Audio Overview biến một công cụ nghiên cứu tương đối “khô” thành sản phẩm dễ demo, dễ chia sẻ và dùng khi đi đường. Mốc 12/2024 và 06–07/2026 lại dịch chuyển theo chiều professional/enterprise: privacy, team corpus, code execution và artifact có thể tải xuống khiến sản phẩm chen sâu vào workflow có ngân sách.

### Switching cost theo 4 forces

| Lực | Biểu hiện với Gemini Notebook |
|---|---|
| **Push — vấn đề với cách hiện tại** | Đọc corpus dài tốn thời gian; tìm kiếm thủ công bỏ sót liên kết; nhiều chatbot không chỉ rõ bằng chứng; workflow bị chia giữa search, notes và công cụ tạo nội dung. |
| **Pull — sức hút của giải pháp mới** | Citation về nguồn, hỏi đáp trên corpus riêng, audio/video/mind map/quiz và artifact đa định dạng; một notebook có thể tái dùng cho nhiều đầu ra. |
| **Habit — quán tính của cách cũ** | Người dùng đã quen Google Search, Drive, Zotero, Notion hoặc ChatGPT; quy trình review nguồn và template nội bộ đã tồn tại. Việc phải tổ chức/upload nguồn vẫn là ma sát. |
| **Anxiety — nỗi lo khi chuyển** | AI có thể hiểu sai hoặc bỏ sót; nguồn web được chọn có thể kém; dữ liệu nhạy cảm và quyền truy cập là rủi ro; output nghe thuyết phục có thể khiến người học bỏ qua bản gốc. |

**Lực giữ mạnh nhất:** pull tích lũy thành switching cost dữ liệu/workflow: corpus đã tuyển chọn, notes, citation và nhiều artifact cùng nằm trong notebook. Nếu notebook trở thành lớp dữ liệu dùng chung giữa Gemini, Search và Workspace, chi phí rời đi càng cao. Nếu đối thủ nhập corpus dễ dàng và cho citation/agent tốt tương đương, lợi thế này giảm nhanh vì model nền và tính năng tạo output có thể bị sao chép.

## §3. Ba dự đoán hướng đi (6–12 tháng tới)

### Dự đoán 1 *(loại: mở rộng tính năng)*

- **Dự đoán:** Gemini Notebook sẽ thêm “research workflow” có trạng thái: người dùng duyệt kế hoạch, giao nhiều bước tìm/đọc/phân tích/tạo artifact, rồi theo dõi evidence và thay đổi đầu ra thay vì chỉ nhận một câu trả lời cuối.
- **Lập luận:** Discover Sources 04/2025 đã kéo job sang thu thập nguồn; nâng cấp agentic 06/2026 đã có cloud computer, code và hơn 100 skill. Bước hợp lý tiếp theo là đóng gói năng lực đó thành quy trình có thể kiểm soát cho analyst/team hiện tại, đồng thời giảm anxiety về agent chạy hộp đen.

### Dự đoán 2 *(loại: mở rộng segment)*

- **Dự đoán:** Google sẽ tung template và connector chuyên ngành trước cho education và knowledge-work có corpus lớn (research, market analysis, sales enablement), kèm policy/permission theo vai trò; chưa đi sâu thành vertical AI “chốt quyết định” ở lĩnh vực rủi ro cao như pháp lý hoặc y khoa.
- **Lập luận:** Từ Plus 12/2024 tới artifact 06/2026, chuỗi mốc liên tục tăng giá trị cho team; tệp hiện tại đã là tổ chức. Nhưng moat hiện tại là horizontal corpus/workflow, chưa có domain expert và liability layer đủ mạnh — vì vậy connector + template là bước mở rộng ít rủi ro hơn một vertical hoàn chỉnh.

### Dự đoán 3 *(loại: mô hình kiếm tiền / đe dọa Big Tech)*

- **Dự đoán:** Tên NotebookLM sẽ tiếp tục mờ đi: notebook trở thành primitive dùng chung trong Gemini, Search và Workspace; tính năng cơ bản được bundle để kéo adoption, còn agent runs, compute, connector doanh nghiệp, quản trị và artifact chất lượng cao nằm ở các gói Pro/Ultra/Workspace.
- **Lập luận:** Plus 12/2024 đã chứng minh willingness-to-pay theo usage và governance; đổi tên + sync 07/2026 cho thấy chiến lược hệ sinh thái. Bundle giúp Google chống việc Gemini tự “ăn” NotebookLM, còn compute và governance tạo thước đo tính phí rõ hơn số tin nhắn.

**Dự đoán tự tin nhất:** Dự đoán 3, vì nó đã có tín hiệu tổ chức và phân phối rõ trong hai mốc 12/2024 và 07/2026. Giả định có thể làm dự đoán gãy là Google thấy thương hiệu NotebookLM có sức hút độc lập đủ lớn và giữ nó tách khỏi Gemini để tránh làm Gemini phức tạp.

## §4. AI Log

| Việc | AI làm hay bạn làm? | Bạn kiểm chứng/phán đoán lại thế nào? |
|---|---|---|
| Đọc đề, chọn NotebookLM và lập danh sách mốc ứng viên | AI đề xuất; người nộp cần xác nhận lựa chọn | Kiểm tra sản phẩm thỏa ba tiêu chí CP0 và mở được nhiều hơn ba nguồn timeline chính thức. |
| Tìm và tóm tắt timeline | AI tìm kiếm và tổng hợp | Mở từng bài Google gốc, đối chiếu ngày, tên tính năng và phạm vi rollout; chỉ giữ tám quyết định làm đổi job, modality, segment hoặc business model. |
| Revert từng mốc về nguyên lý | AI soạn phân tích ban đầu | Không dùng nhãn “tăng trưởng/retention”; kiểm tra mỗi hàng có nguyên lý gọi tên được và giải thích được quan hệ nhân–quả. Người nộp cần tự đọc lại và sửa theo nội dung buổi học nếu cách gọi framework khác. |
| Phân tích user, JTBD và 4 forces | AI tổng hợp từ thông điệp sản phẩm, use case và phản hồi công khai | Viết JTBD theo công việc cần hoàn thành, không theo feature; nối segment shift về mốc cụ thể. Người nộp cần tự thử free tier với một bộ tài liệu thật để xác nhận ma sát upload, citation và chất lượng output. |
| Viết ba dự đoán | AI đề xuất; phán đoán cuối cần thuộc về người nộp | Mỗi dự đoán được dẫn ngược về ít nhất hai quan sát ở §1–§2 và nêu giả định làm dự đoán gãy. |
| Biên tập memo | AI thực hiện bản nháp | Người nộp đọc toàn bài, kiểm tra mọi link, bổ sung họ tên/MSV và chỉ giữ những nhận định mình có thể tự bảo vệ khi bị phản biện. |

## Checklist trước khi nộp

- [x] Timeline có 8 mốc lớn, mỗi mốc có link nguồn gốc và nguyên lý có tên.
- [x] Early adopters, tệp hiện tại, JTBD và 4 forces đủ cụ thể; segment shift nối về timeline.
- [x] Ba dự đoán cụ thể, có lập luận từ §1–§2 và có giả định phản biện.
- [x] AI log có ít nhất ba hàng và khai rõ phần AI/phần người nộp.
- [ ] Người nộp đã tự mở lại toàn bộ link và dùng thử NotebookLM/Gemini Notebook với corpus thật.
- [x] Đã điền họ tên và mã sinh viên.
- [ ] Repo public đúng tên `Day16-Track1-2A202602705-TranVuGiaHuy` và đã upload `memo.md`.
