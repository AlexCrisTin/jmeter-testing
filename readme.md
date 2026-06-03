Dựa trên kết quả bạn gửi, bạn có thể viết phần **Kết quả kiểm thử hiệu năng bằng JMeter** như sau.

---

# Kiểm thử hiệu năng website YouTube bằng Apache JMeter

## 1. Mục tiêu kiểm thử

Bài kiểm thử được thực hiện nhằm đánh giá thời gian phản hồi của một số thao tác cơ bản trên website YouTube bằng công cụ Apache JMeter. Các thao tác được kiểm thử bao gồm:

| STT | Chức năng kiểm thử     | Mô tả                                                           |
| --- | ---------------------- | --------------------------------------------------------------- |
| 1   | Open YouTube Home Page | Kiểm tra thời gian phản hồi khi truy cập trang chủ YouTube      |
| 2   | Open Video Page        | Kiểm tra thời gian phản hồi khi mở một trang video trên YouTube |

Do YouTube là một website thật và có lượng người dùng lớn, bài kiểm thử chỉ được thực hiện với mức tải thấp nhằm mục đích học tập, không nhằm tạo tải lớn lên hệ thống.

---

## 2. Cấu hình kiểm thử

Trong JMeter, bài kiểm thử được cấu hình với các thành phần chính sau:

| Thành phần            | Mô tả                                                      |
| --------------------- | ---------------------------------------------------------- |
| Thread Group          | Tạo người dùng ảo để gửi request đến website               |
| HTTP Request Defaults | Cấu hình protocol HTTPS và server `www.youtube.com`        |
| HTTP Header Manager   | Bổ sung thông tin header như User-Agent và Accept-Language |
| HTTP Request          | Gửi request đến trang chủ và trang video YouTube           |
| View Results Tree     | Kiểm tra trạng thái từng request                           |
| Graph Results         | Quan sát biểu đồ hiệu năng                                 |
| Aggregate Graph       | Tổng hợp số liệu kiểm thử                                  |

Tổng số request được thực hiện là **100 samples**, trong đó mỗi chức năng có **50 samples**.

---

## 3. Kết quả kiểm thử

Kết quả thu được từ **Aggregate Graph** trong JMeter như sau:

| Label                  | Samples | Average |  Median | 90% Line | 95% Line | 99% Line |    Min |     Max | Error % | Throughput |
| ---------------------- | ------: | ------: | ------: | -------: | -------: | -------: | -----: | ------: | ------: | ---------: |
| Open YouTube Home Page |      50 | 1136 ms |  513 ms |  3347 ms |  3381 ms |  3815 ms | 350 ms | 3815 ms |   0.00% |    4.2/sec |
| Open Video Page        |      50 | 1194 ms | 1164 ms |  1464 ms |  1630 ms |  2508 ms | 464 ms | 2508 ms |   0.00% |    4.2/sec |
| TOTAL                  |     100 | 1165 ms | 1055 ms |  2417 ms |  3347 ms |  3794 ms | 350 ms | 3815 ms |   0.00% |    6.7/sec |

Ngoài ra, trong phần **Graph Results**, JMeter ghi nhận:

| Chỉ số         |        Giá trị |
| -------------- | -------------: |
| No. of Samples |            100 |
| Latest Sample  |         895 ms |
| Average        |        1165 ms |
| Median         |        1055 ms |
| Deviation      |         797 ms |
| Throughput     | 408.960/minute |

---

## 4. Nhận xét kết quả

Dựa trên kết quả kiểm thử, cả hai request đều thực hiện thành công với tỷ lệ lỗi là **0.00%**. Điều này cho thấy trong quá trình kiểm thử, các request gửi đến YouTube đều nhận được phản hồi hợp lệ.

Request **Open YouTube Home Page** có thời gian phản hồi trung bình là **1136 ms**, trong khi request **Open Video Page** có thời gian phản hồi trung bình là **1194 ms**. Như vậy, trang video có thời gian phản hồi trung bình cao hơn trang chủ một chút. Điều này là hợp lý vì trang video thường chứa nhiều nội dung hơn, bao gồm thông tin video, đề xuất video, bình luận và các tài nguyên liên quan.

Chỉ số **Median** của trang chủ là **513 ms**, thấp hơn khá nhiều so với Average **1136 ms**. Điều này cho thấy phần lớn request trang chủ phản hồi khá nhanh, nhưng có một số request mất thời gian lâu hơn, làm tăng giá trị trung bình. Giá trị lớn nhất của trang chủ là **3815 ms**, đây là request chậm nhất trong toàn bộ bài kiểm thử.

Đối với trang video, Median là **1164 ms**, gần với Average là **1194 ms**, cho thấy thời gian phản hồi của các request mở video ổn định hơn so với request truy cập trang chủ. Giá trị lớn nhất của request mở video là **2508 ms**, thấp hơn giá trị lớn nhất của trang chủ.

Tổng thể, thời gian phản hồi trung bình của toàn bộ bài kiểm thử là **1165 ms**, tương đương khoảng **1.165 giây**. Throughput tổng là **6.7 request/giây**, cho thấy JMeter đã gửi và nhận phản hồi ổn định trong quá trình kiểm thử.

---

## 5. Đánh giá

Kết quả kiểm thử cho thấy website YouTube phản hồi tốt trong điều kiện kiểm thử nhẹ. Tỷ lệ lỗi bằng **0.00%** cho thấy không có request nào bị lỗi trong quá trình chạy test. Thời gian phản hồi trung bình khoảng hơn 1 giây là chấp nhận được đối với một website có nhiều nội dung động như YouTube.

Tuy nhiên, có sự dao động về thời gian phản hồi giữa các request, thể hiện qua chỉ số **Deviation = 797 ms**. Nguyên nhân có thể đến từ tốc độ mạng, vị trí máy chủ, cơ chế cache, nội dung động của YouTube hoặc thời điểm thực hiện kiểm thử.

---

## 6. Hạn chế của bài kiểm thử

Bài kiểm thử này chỉ đánh giá hiệu năng ở mức **HTTP request**, không đánh giá toàn bộ trải nghiệm người dùng trên trình duyệt thật. Apache JMeter không render giao diện, không chạy JavaScript và không đo đầy đủ thời gian tải hình ảnh, video, CSS hoặc các thành phần động như một trình duyệt thực tế.

Ngoài ra, YouTube là một website thật nên bài kiểm thử không thực hiện kiểm thử tải lớn. Số lượng request được giới hạn ở mức thấp để đảm bảo mục đích học tập và tránh ảnh hưởng đến hệ thống thật.

---

## 7. Kết luận

Qua bài kiểm thử, có thể thấy Apache JMeter hỗ trợ tốt việc tạo kịch bản kiểm thử hiệu năng, gửi HTTP request, thu thập số liệu và hiển thị kết quả dưới dạng bảng và biểu đồ. Kết quả kiểm thử YouTube cho thấy các request đều thành công, tỷ lệ lỗi bằng **0.00%**, thời gian phản hồi trung bình toàn bài test là **1165 ms**.

Bài kiểm thử giúp người thực hiện hiểu được cách sử dụng JMeter để đánh giá các chỉ số hiệu năng cơ bản như Average, Median, Min, Max, Error %, Throughput và các percentile như 90%, 95%, 99%. Tuy nhiên, để đánh giá đầy đủ hiệu năng giao diện người dùng, cần kết hợp thêm các công cụ kiểm thử trình duyệt thật như Selenium, Playwright hoặc Lighthouse.
