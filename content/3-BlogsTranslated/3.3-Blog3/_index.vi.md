---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Chiến lược tối ưu hóa chi phí hiệu quả cho Amazon Bedrock
bởi Biswanath Mukherjee và Upendra V vào ngày 10 tháng 6 năm 2025 trong [Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Thực tiễn tốt nhất](https://aws.amazon.com/blogs/machine-learning/category/post-types/best-practices/), [Tối ưu hóa chi phí đám mây](https://aws.amazon.com/blogs/machine-learning/category/business-intelligence/cloud-cost-optimization/), [AI tạo sinh](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), [AI tạo sinh*](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/) Permalink  Bình luận  Chia sẻ
 
---

Khách hàng ngày càng sử dụng [AI tạo sinh](https://aws.amazon.com/ai/generative-ai/) để nâng cao hiệu quả, cá nhân hóa trải nghiệm và thúc đẩy đổi mới trên nhiều ngành công nghiệp. Ví dụ, AI tạo sinh có thể được sử dụng để thực hiện tóm tắt văn bản, hỗ trợ các chiến lược tiếp thị cá nhân hóa, tạo ra các trợ lý chat quan trọng cho doanh nghiệp, v.v. Tuy nhiên, khi việc áp dụng AI tạo sinh tăng lên, các chi phí liên quan có thể leo thang trong một số lĩnh vực bao gồm chi phí suy luận, triển khai và tùy chỉnh mô hình. Tối ưu hóa chi phí hiệu quả có thể giúp đảm bảo rằng các sáng kiến AI tạo sinh vẫn bền vững về mặt tài chính và mang lại lợi tức đầu tư tích cực. Quản lý chi phí chủ động giúp các doanh nghiệp có được tiềm năng biến đổi tốt nhất của AI tạo sinh trong khi duy trì sức khỏe tài chính của họ.

[Amazon Bedrock](https://aws.amazon.com/bedrock/) là một dịch vụ được quản lý đầy đủ cung cấp sự lựa chọn các [mô hình nền tảng](https://aws.amazon.com/what-is/foundation-models/) (FM) hiệu suất cao từ các công ty AI hàng đầu như AI21 Labs, Anthropic, Cohere, DeepSeek, Luma, Meta, Mistral AI, Stability AI, và Amazon thông qua một API duy nhất, cùng với một bộ khả năng rộng lớn bạn cần để xây dựng các ứng dụng AI tạo sinh với bảo mật, quyền riêng tư và [AI có trách nhiệm](https://aws.amazon.com/ai/responsible-ai/). Sử dụng Amazon Bedrock, bạn có thể thử nghiệm và đánh giá các FM hàng đầu cho trường hợp sử dụng của mình, tùy chỉnh riêng tư chúng với dữ liệu của bạn bằng các kỹ thuật như fine-tuning và [Retrieval Augmented Generation (RAG)](https://aws.amazon.com/what-is/retrieval-augmented-generation/), và xây dựng các agent thực thi các tác vụ bằng cách sử dụng hệ thống doanh nghiệp và nguồn dữ liệu của bạn.

Với việc áp dụng Amazon Bedrock ngày càng tăng, tối ưu hóa chi phí là điều bắt buộc để giúp giữ các chi phí liên quan đến triển khai và chạy ứng dụng AI tạo sinh có thể quản lý được và phù hợp với ngân sách của tổ chức bạn. Trong bài viết này, bạn sẽ học về các kỹ thuật tối ưu hóa chi phí chiến lược khi sử dụng Amazon Bedrock.

---

## Hiểu về giá cả Amazon Bedrock
Amazon Bedrock cung cấp một mô hình giá cả toàn diện dựa trên việc sử dụng thực tế các FM và các dịch vụ liên quan. Các thành phần giá cả cốt lõi bao gồm suy luận mô hình (có sẵn trong các tùy chọn On-Demand, Batch, và Provisioned Throughput), tùy chỉnh mô hình (tính phí cho training, lưu trữ và suy luận), và Custom Model Import (nhập miễn phí nhưng tính phí cho suy luận và lưu trữ). Thông qua [Amazon Bedrock Marketplace](https://aws.amazon.com/bedrock/marketplace/), bạn có thể truy cập hơn 100 mô hình với các cấu trúc giá cả khác nhau cho các mô hình độc quyền và công cộng. Bạn có thể xem [giá cả Amazon Bedrock](https://aws.amazon.com/bedrock/pricing/) để có cái nhìn tổng quan về giá cả và thông tin chi tiết hơn về các mô hình giá cả.

## Giám sát chi phí trong Amazon Bedrock
Bạn có thể giám sát chi phí sử dụng Amazon Bedrock bằng các cách tiếp cận sau:

- Hồ sơ suy luận ứng dụng – Amazon Bedrock cung cấp hồ sơ suy luận ứng dụng mà bạn có thể sử dụng để áp dụng [thẻ phân bổ chi phí](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) tùy chỉnh để theo dõi, quản lý và kiểm soát chi phí và sử dụng FM theo yêu cầu trên các workload và tenant khác nhau.
- Gắn thẻ phân bổ chi phí – Bạn có thể [gắn thẻ tất cả các mô hình Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/tagging.html), căn chỉnh việc sử dụng với các taxonomy tổ chức cụ thể như trung tâm chi phí, đơn vị kinh doanh, nhóm, và ứng dụng để theo dõi chi phí chính xác. Để thực hiện các hoạt động gắn thẻ, bạn cần [Amazon Resource Name](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html) (ARN) của tài nguyên mà bạn muốn thực hiện hoạt động gắn thẻ.
- Tích hợp với các công cụ chi phí AWS – Giám sát chi phí Amazon Bedrock tích hợp với [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/), [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/), [AWS Cost and Usage Reports](https://aws.amazon.com/aws-cost-management/aws-cost-and-usage-reporting/), và [AWS Cost Anomaly Detection](https://aws.amazon.com/aws-cost-management/aws-cost-anomaly-detection/), [cho phép các tổ chức thiết lập ngân sách dựa trên thẻ, nhận cảnh báo cho ngưỡng sử dụng và phát hiện các pattern chi tiêu bất thường](https://aws.amazon.com/blogs/machine-learning/track-allocate-and-manage-your-generative-ai-cost-and-usage-with-amazon-bedrock/)
- Giám sát metrics [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) – Các tổ chức có thể sử dụng [Amazon CloudWatch để giám sát metrics thời gian chạy cho các ứng dụng Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/monitoring.html) theo hồ sơ suy luận, đặt cảnh báo dựa trên ngưỡng, và nhận thông báo để quản lý thời gian thực việc sử dụng tài nguyên và chi phí. Bạn có thể giám sát tất cả các phần của ứng dụng Amazon Bedrock bằng Amazon CloudWatch, thu thập dữ liệu thô và xử lý nó thành các metrics có thể đọc được, gần như thời gian thực. Bạn có thể vẽ đồ thị các metrics bằng [AWS Management Console](https://aws.amazon.com/console/) cho CloudWatch. Bạn cũng có thể đặt cảnh báo theo dõi các ngưỡng nhất định và gửi thông báo hoặc thực hiện hành động khi giá trị vượt quá những ngưỡng đó.
- Khả năng hiển thị cụ thể theo tài nguyên – CloudWatch cung cấp các metrics như Invocations, InvocationLatency, InputTokenCount, OutputTokenCount, và nhiều metrics lỗi khác có thể được lọc theo ID mô hình và các chiều khác để giám sát chi tiết việc sử dụng và hiệu suất Amazon Bedrock.

## Chiến lược tối ưu hóa chi phí cho Amazon Bedrock
Khi xây dựng các ứng dụng AI tạo sinh với Amazon Bedrock, việc triển khai các chiến lược tối ưu hóa chi phí chu đáo có thể giảm đáng kể chi phí của bạn trong khi duy trì hiệu suất ứng dụng. Trong phần này, bạn sẽ tìm thấy các cách tiếp cận chính để xem xét theo thứ tự sau:

1. Chọn mô hình phù hợp
2. Xác định xem có cần tùy chỉnh không
   a. Nếu có, khám phá các tùy chọn theo thứ tự đúng
   b. Nếu không, tiến hành bước tiếp theo
3. Thực hiện kỹ thuật và quản lý prompt
4. Thiết kế các agent hiệu quả
5. Chọn tùy chọn tiêu thụ đúng

Luồng này được hiển thị trong sơ đồ luồng sau.

![ picture](/images/3-BlogsTranslated/image9.png)

## Chọn một mô hình phù hợp cho trường hợp sử dụng của bạn

Amazon Bedrock cung cấp quyền truy cập vào [danh mục FM đa dạng](https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html) thông qua một API duy nhất. Dịch vụ liên tục mở rộng các dịch vụ của mình với các mô hình và nhà cung cấp mới, mỗi cái có cấu trúc giá cả và khả năng khác nhau.

Ví dụ, hãy xem xét sự khác biệt giá cả theo yêu cầu giữa [các mô hình Amazon Nova](https://aws.amazon.com/nova/) trong [AWS Region](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region) US East (Ohio). Giá này là hiện tại tính đến ngày 21 tháng 5 năm 2025. Tham khảo trang [giá cả Amazon Bedrock](https://aws.amazon.com/bedrock/pricing/) để có dữ liệu mới nhất.

Như được hiển thị trong bảng sau, giá cả khác biệt đáng kể giữa các mô hình Amazon Nova Micro, Amazon Nova Lite, và Amazon Nova Pro. Ví dụ, Amazon Nova Micro rẻ hơn khoảng 1.71 lần so với Amazon Nova Lite dựa trên mỗi 1.000 token đầu vào tại thời điểm viết bài này. Nếu bạn không cần khả năng đa phương thức và độ chính xác của Amazon Nova Micro đáp ứng trường hợp sử dụng của bạn, thì bạn không cần chọn Amazon Nova Lite. Điều này cho thấy tại sao việc chọn mô hình phù hợp cho trường hợp sử dụng của bạn là quan trọng. Mô hình lớn nhất hoặc tiên tiến nhất không phải lúc nào cũng cần thiết cho mọi ứng dụng.

### Bảng so sánh giá Amazon Nova models

| Amazon Nova models | Giá mỗi 1.000 token đầu vào | Giá mỗi 1.000 token đầu ra |
|-------------------|---------------------------|---------------------------|
| Amazon Nova Micro | $0.000035 | $0.00014 |
| Amazon Nova Lite | $0.00006 | $0.00024 |
| Amazon Nova Pro | $0.0008 | $0.0032 |

Một trong những lợi thế chính của Amazon Bedrock là API thống nhất, trừu tượng hóa sự phức tạp của việc làm việc với các mô hình khác nhau. Bạn có thể chuyển đổi giữa các mô hình bằng cách thay đổi ID mô hình trong yêu cầu của bạn với những thay đổi mã tối thiểu. Với sự linh hoạt này, bạn có thể chọn mô hình được tối ưu hóa chi phí và hiệu suất nhất đáp ứng yêu cầu của bạn và chỉ nâng cấp khi cần thiết.

* Thực tiễn tốt nhất: Sử dụng các tính năng gốc của Amazon Bedrock để đánh giá [hiệu suất của mô hình nền tảng](https://docs.aws.amazon.com/bedrock/latest/userguide/evaluation.html) cho trường hợp sử dụng của bạn. Bắt đầu với một công việc đánh giá mô hình tự động để thu hẹp phạm vi. Tiếp theo bằng cách sử dụng [LLM làm thẩm phán](https://docs.aws.amazon.com/bedrock/latest/userguide/evaluation-judge.html) hoặc [đánh giá dựa trên con người](https://docs.aws.amazon.com/bedrock/latest/userguide/evaluation-human.html) theo yêu cầu cho trường hợp sử dụng của bạn.

## Thực hiện tùy chỉnh mô hình theo thứ tự đúng
Khi tùy chỉnh FM trong Amazon Bedrock để cung cấp ngữ cảnh cho các phản hồi, việc chọn chiến lược theo thứ tự đúng có thể giảm đáng kể chi phí của bạn trong khi tối đa hóa hiệu suất. Bạn có bốn chiến lược chính có sẵn, mỗi cái có tác động chi phí khác nhau:

1.[Kỹ thuật Prompt](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-engineering-guidelines.html) – Bắt đầu bằng cách tạo ra các prompt chất lượng cao hiệu quả điều kiện hóa mô hình để tạo ra các phản hồi mong muốn. Cách tiếp cận này yêu cầu tài nguyên tối thiểu và không có chi phí cơ sở hạ tầng bổ sung ngoài các cuộc gọi suy luận tiêu chuẩn của bạn.
2.[RAG – Amazon Bedrock Knowledge Bases](https://aws.amazon.com/bedrock/knowledge-bases/) là một tính năng được quản lý đầy đủ với quản lý ngữ cảnh phiên tích hợp sẵn và ghi công nguồn giúp bạn triển khai toàn bộ quy trình RAG từ ingestion đến retrieval và prompt augmentation mà không cần xây dựng các tích hợp tùy chỉnh với nguồn dữ liệu và quản lý luồng dữ liệu.
3.[Fine-tuning](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-model-fine-tuning.html) – Cách tiếp cận này bao gồm việc cung cấp dữ liệu training được gắn nhãn để cải thiện hiệu suất mô hình trên các tác vụ cụ thể. Mặc dù hiệu quả, fine-tuning yêu cầu tài nguyên tính toán bổ sung và tạo ra các phiên bản mô hình tùy chỉnh với chi phí hosting liên quan.
4.[Continued pre-training](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-model-fine-tuning.html) – Tùy chọn tốn nhiều tài nguyên nhất bao gồm việc cung cấp dữ liệu không gắn nhãn để tiếp tục training FM trên nội dung cụ thể theo domain. Cách tiếp cận này phát sinh chi phí cao nhất và thời gian triển khai dài nhất.

Biểu đồ sau đây cho thấy sự leo thang của độ phức tạp, chất lượng, chi phí và thời gian của bốn cách tiếp cận này.
![ picture](/images/3-BlogsTranslated/image10.png)

Thực tiễn tốt nhất: Triển khai những chiến lược này một cách tiệm tiến. Bắt đầu với kỹ thuật prompt làm nền tảng—nó hiệu quả về chi phí và thường có thể mang lại kết quả ấn tượng với đầu tư tối thiểu. Tham khảo phần Tối ưu hóa cho prompt rõ ràng và súc tích để học về các chiến lược khác nhau mà bạn có thể tuân theo để viết prompt tốt. Tiếp theo, tích hợp RAG khi bạn cần kết hợp thông tin độc quyền vào các phản hồi. Hai cách tiếp cận này cùng nhau nên giải quyết hầu hết các trường hợp sử dụng trong khi duy trì cấu trúc chi phí hiệu quả. Khám phá fine-tuning và continued pre-training chỉ khi bạn có các yêu cầu cụ thể không thể được giải quyết thông qua hai phương pháp đầu tiên và trường hợp sử dụng của bạn biện minh cho chi phí bổ sung.

Bằng cách tuân theo hệ thống phân cấp triển khai này, được hiển thị trong hình sau, bạn có thể tối ưu hóa cả hiệu suất Amazon Bedrock và phân bổ ngân sách của mình. Đây là mô hình tinh thần cấp cao để chọn các tùy chọn khác nhau:

![ picture](/images/3-BlogsTranslated/image10.png)

## Sử dụng tính năng chưng cất mô hình gốc của Amazon Bedrock
[Amazon Bedrock Model Distillation](https://aws.amazon.com/bedrock/model-distillation/) là một tính năng mạnh mẽ mà bạn có thể sử dụng để truy cập các mô hình nhỏ hơn, hiệu quả về chi phí hơn mà không hy sinh hiệu suất và độ chính xác cho các trường hợp sử dụng cụ thể của bạn.

- Nâng cao độ chính xác của các mô hình (học sinh) hiệu quả về chi phí nhỏ hơn – Với Amazon Bedrock Model Distillation, bạn có thể chọn một mô hình giáo viên có độ chính xác mà bạn muốn đạt được cho trường hợp sử dụng của mình và sau đó chọn một mô hình học sinh mà bạn muốn fine-tune. Model distillation tự động hóa quá trình tạo ra các phản hồi từ giáo viên và sử dụng những phản hồi đó để fine-tune mô hình học sinh.
- Tối đa hóa hiệu suất mô hình chưng cất với tổng hợp dữ liệu độc quyền – Fine-tuning một mô hình nhỏ hơn, hiệu quả về chi phí để đạt được độ chính xác tương tự như một mô hình lớn hơn cho trường hợp sử dụng cụ thể của bạn là một quá trình lặp đi lặp lại. Để loại bỏ một số gánh nặng lặp lại cần thiết để đạt được kết quả tốt hơn, Amazon Bedrock Model Distillation có thể chọn áp dụng các phương pháp tổng hợp dữ liệu khác nhau phù hợp nhất với trường hợp sử dụng của bạn. Ví dụ, Amazon Bedrock có thể mở rộng bộ dữ liệu training bằng cách tạo ra các prompt tương tự, hoặc nó có thể tạo ra các phản hồi tổng hợp chất lượng cao sử dụng các cặp prompt-response do khách hàng cung cấp làm ví dụ vàng.
- Giảm chi phí bằng cách mang dữ liệu sản xuất của bạn – Với fine-tuning truyền thống, bạn được yêu cầu tạo prompt và phản hồi. Với Amazon Bedrock Model Distillation, bạn chỉ cần cung cấp prompt, được sử dụng để tạo ra các phản hồi tổng hợp và fine-tune các mô hình học sinh.

Thực tiễn tốt nhất: Xem xét model distillation khi bạn có một trường hợp sử dụng cụ thể, được định nghĩa rõ ràng nơi một mô hình lớn hơn hoạt động tốt nhưng chi phí nhiều hơn mong muốn. Cách tiếp cận này đặc biệt có giá trị cho các kịch bản suy luận khối lượng lớn nơi việc tiết kiệm chi phí liên tục sẽ nhanh chóng bù đắp cho khoản đầu tư ban đầu vào chưng cất.

## Sử dụng định tuyến prompt thông minh của Amazon Bedrock
Với [Amazon Bedrock Intelligent Prompt Routing](https://aws.amazon.com/bedrock/intelligent-prompt-routing/), bây giờ bạn có thể sử dụng sự kết hợp của các FM từ cùng một họ mô hình để giúp tối ưu hóa chất lượng và chi phí khi gọi một mô hình. Ví dụ, bạn có thể định tuyến giữa họ mô hình Claude của Anthropic—giữa Claude 3.5 Sonnet và Claude 3 Haiku tùy thuộc vào độ phức tạp của prompt. Điều này đặc biệt hữu ích cho các ứng dụng như trợ lý dịch vụ khách hàng, nơi các truy vấn không phức tạp có thể được xử lý bởi các mô hình nhỏ hơn, nhanh hơn và hiệu quả về chi phí hơn, và các truy vấn phức tạp được định tuyến đến các mô hình có khả năng hơn. Định tuyến prompt thông minh có thể giảm chi phí lên đến 30% mà không ảnh hưởng đến độ chính xác.

Thực tiễn tốt nhất: Triển khai [định tuyến prompt thông minh](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-routing.html) cho các ứng dụng xử lý một loạt rộng các độ phức tạp truy vấn.

## Tối ưu hóa cho prompt rõ ràng và súc tích
Tối ưu hóa prompt để rõ ràng và súc tích trong Amazon Bedrock tập trung vào giao tiếp có cấu trúc, hiệu quả với mô hình để giảm thiểu việc sử dụng token và tối đa hóa chất lượng phản hồi. Thông qua các kỹ thuật như hướng dẫn rõ ràng, định dạng đầu ra cụ thể và định nghĩa vai trò chính xác, bạn có thể đạt được kết quả tốt hơn trong khi giảm chi phí liên quan đến tiêu thụ token.

- Hướng dẫn có cấu trúc – Chia nhỏ [prompt phức tạp thành các bước rõ ràng, được đánh số hoặc bullet point.](https://docs.aws.amazon.com/bedrock/latest/userguide/design-a-prompt.html#prompt-instructions) Điều này giúp mô hình tuân theo một chuỗi logic và cải thiện tính nhất quán của các phản hồi trong khi giảm việc sử dụng token.
- Đặc tả đầu ra – Định nghĩa rõ ràng [định dạng và ràng buộc mong muốn cho phản hồi.](https://docs.aws.amazon.com/bedrock/latest/userguide/design-a-prompt.html#prompt-output-indicators) Ví dụ, chỉ định giới hạn từ, yêu cầu định dạng, hoặc sử dụng các chỉ thị như "Vui lòng cung cấp một bản tóm tắt ngắn gọn trong 2-3 câu" để kiểm soát độ dài đầu ra.
- Tránh dư thừa – Loại bỏ ngữ cảnh không cần thiết và hướng dẫn lặp lại. Giữ prompt tập trung vào thông tin và yêu cầu thiết yếu vì nội dung thừa có thể tăng chi phí và có thể làm mô hình bối rối.
- Sử dụng dấu phân cách – [Sử dụng các dấu phân cách rõ ràng](https://docs.aws.amazon.com/bedrock/latest/userguide/design-a-prompt.html#prompt-separators) (như dấu ngoặc kép ba, dấu gạch ngang, hoặc thẻ kiểu XML) để tách các phần khác nhau của prompt giúp mô hình phân biệt giữa ngữ cảnh, hướng dẫn và ví dụ.
- Độ chính xác vai trò và ngữ cảnh – Bắt đầu với định nghĩa vai trò rõ ràng và ngữ cảnh cụ thể liên quan đến tác vụ. Ví dụ, "Bạn là một chuyên gia tài liệu kỹ thuật tập trung vào việc giải thích các khái niệm phức tạp bằng thuật ngữ đơn giản" cung cấp hướng dẫn tốt hơn so với mô tả vai trò chung chung.

Thực tiễn tốt nhất: Amazon Bedrock cung cấp một tính năng được quản lý đầy đủ để [tối ưu hóa prompt](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management-optimize.html) cho một mô hình được chọn. Điều này giúp giảm chi phí bằng cách cải thiện hiệu quả và hiệu suất prompt, dẫn đến kết quả tốt hơn với ít token hơn và ít lời gọi mô hình hơn. Tính năng tối ưu hóa prompt tự động tinh chỉnh prompt của bạn để tuân theo các thực tiễn tốt nhất cho từng mô hình cụ thể, loại bỏ nhu cầu kỹ thuật prompt thủ công mở rộng có thể mất hàng tháng thử nghiệm. Sử dụng tính năng tối ưu hóa prompt tích hợp sẵn này trong Amazon Bedrock để bắt đầu và tối ưu hóa thêm để có kết quả tốt hơn khi cần. Thử nghiệm với prompt để làm cho chúng rõ ràng và súc tích để giảm số lượng token mà không ảnh hưởng đến chất lượng phản hồi.

## Tối ưu hóa chi phí và hiệu suất bằng cách sử dụng bộ nhớ đệm prompt Amazon Bedrock
Bạn có thể sử dụng [bộ nhớ đệm prompt](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-caching.html) với các mô hình được hỗ trợ trên Amazon Bedrock để giảm độ trễ phản hồi suy luận và chi phí token đầu vào. Bằng cách thêm các phần ngữ cảnh của bạn vào bộ nhớ đệm, mô hình có thể sử dụng bộ nhớ đệm để bỏ qua tính toán lại đầu vào, cho phép Amazon Bedrock chia sẻ trong việc tiết kiệm tính toán và giảm độ trễ phản hồi của bạn.

- Giảm chi phí đáng kể – Bộ nhớ đệm prompt có thể giảm chi phí lên đến 90% so với chi phí suy luận mô hình tiêu chuẩn, vì các token được cache được tính phí với mức giá giảm so với các token đầu vào không được cache.
- Trường hợp sử dụng lý tưởng – Bộ nhớ đệm prompt đặc biệt có giá trị cho các ứng dụng với ngữ cảnh dài và lặp lại, chẳng hạn như hệ thống Q&A tài liệu nơi người dùng đặt nhiều câu hỏi về cùng một tài liệu hoặc trợ lý mã hóa duy trì ngữ cảnh về các tệp mã.
- Cải thiện độ trễ – Triển khai bộ nhớ đệm prompt có thể giảm độ trễ phản hồi lên đến 85% cho các mô hình được hỗ trợ bằng cách loại bỏ nhu cầu xử lý lại nội dung đã thấy trước đó, làm cho ứng dụng phản hồi nhanh hơn.
- Thời gian lưu giữ bộ nhớ đệm – Nội dung được cache vẫn có sẵn trong tối đa 5 phút sau mỗi lần truy cập, với bộ đếm thời gian reset khi mỗi lần cache hit thành công, làm cho nó lý tưởng cho các cuộc trò chuyện nhiều lượt về cùng ngữ cảnh.
- Cách tiếp cận triển khai – Để triển khai bộ nhớ đệm prompt, các developer xác định các phần prompt được tái sử dụng thường xuyên, gắn thẻ những phần này bằng khối cachePoint trong các cuộc gọi API, và giám sát các metrics sử dụng cache (cacheReadInputTokenCount và cacheWriteInputTokenCount) trong metadata phản hồi để tối ưu hóa hiệu suất.

Thực tiễn tốt nhất: Bộ nhớ đệm prompt có giá trị trong các kịch bản nơi ứng dụng xử lý lặp đi lặp lại cùng ngữ cảnh, chẳng hạn như hệ thống Q&A tài liệu nơi nhiều người dùng truy vấn cùng nội dung. Kỹ thuật này mang lại lợi ích tối đa khi xử lý các ngữ cảnh ổn định không thay đổi thường xuyên, các cuộc trò chuyện nhiều lượt về thông tin giống hệt nhau, các ứng dụng yêu cầu thời gian phản hồi nhanh, các dịch vụ khối lượng lớn với các yêu cầu lặp lại, hoặc các hệ thống nơi tối ưu hóa chi phí là quan trọng mà không hy sinh hiệu suất mô hình.

## Cache prompt trong ứng dụng client
Cache prompt phía client giúp giảm chi phí bằng cách lưu trữ các prompt và phản hồi được sử dụng thường xuyên cục bộ trong ứng dụng của bạn. Cách tiếp cận này giảm thiểu các cuộc gọi API đến các mô hình Amazon Bedrock, dẫn đến tiết kiệm chi phí đáng kể và cải thiện hiệu suất ứng dụng.

- Triển khai lưu trữ cục bộ – Triển khai một cơ chế caching trong ứng dụng của bạn để lưu trữ các prompt phổ biến và phản hồi tương ứng của chúng, sử dụng các kỹ thuật như caching in-memory (Redis, Memcached) hoặc [hệ thống caching cấp ứng dụng](https://aws.amazon.com/caching/general-cache/).
- Tối ưu hóa cache hit – Trước khi thực hiện cuộc gọi API đến Amazon Bedrock, kiểm tra xem prompt hoặc các biến thể tương tự có tồn tại trong cache cục bộ không. Điều này giảm số lượng cuộc gọi API có thể tính phí đến các FM, tác động trực tiếp đến chi phí. Bạn có thể kiểm tra [Thực tiễn tốt nhất Caching](https://aws.amazon.com/caching/best-practices/) để học thêm.
- Chiến lược hết hạn – Triển khai chiến lược hết hạn cache dựa trên thời gian như Time To Live (TTL) để giúp đảm bảo rằng các phản hồi được cache vẫn phù hợp trong khi duy trì lợi ích chi phí. Điều này phù hợp với cửa sổ cache 5 phút được sử dụng bởi Amazon Bedrock để tiết kiệm chi phí tối ưu.
- Cách tiếp cận caching hybrid – Kết hợp caching phía client với bộ nhớ đệm prompt tích hợp sẵn của Amazon Bedrock để tối ưu hóa chi phí tối đa. Sử dụng cache cục bộ cho các kết quả khớp chính xác và cache Amazon Bedrock để tái sử dụng ngữ cảnh một phần.
- Giám sát cache – Triển khai giám sát tỷ lệ cache hit:miss để liên tục tối ưu hóa chiến lược caching của bạn và xác định cơ hội giảm chi phí thêm thông qua việc tái sử dụng prompt được cache.

Thực tiễn tốt nhất: Trong các hệ thống quan trọng về hiệu suất và trang web lưu lượng cao, caching phía client nâng cao thời gian phản hồi và trải nghiệm người dùng trong khi giảm thiểu sự phụ thuộc vào các tương tác API Amazon Bedrock đang diễn ra.

## Xây dựng các agent nhỏ và tập trung tương tác với nhau thay vì một agent đơn lẻ lớn
Tạo ra các agent nhỏ, chuyên biệt tương tác với nhau trong Amazon Bedrock có thể dẫn đến tiết kiệm chi phí đáng kể trong khi cải thiện chất lượng giải pháp. Cách tiếp cận này sử dụng khả năng [hợp tác đa agent](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-multi-agent-collaboration.html) của Amazon Bedrock để xây dựng các ứng dụng AI tạo sinh hiệu quả và hiệu quả về chi phí hơn.

Lợi thế kiến trúc đa agent: Bạn có thể sử dụng hợp tác đa agent Amazon Bedrock để điều phối nhiều agent AI chuyên biệt làm việc cùng nhau để giải quyết các vấn đề kinh doanh phức tạp. Bằng cách tạo ra các agent nhỏ hơn, được xây dựng có mục đích thay vì một agent lớn duy nhất, bạn có thể:

- Tối ưu hóa lựa chọn mô hình dựa trên các tác vụ cụ thể – Sử dụng các FM kinh tế hơn cho các tác vụ đơn giản và dự trữ các mô hình premium cho các tác vụ suy luận phức tạp
- Cho phép xử lý song song – Nhiều agent chuyên biệt có thể làm việc đồng thời trên các khía cạnh khác nhau của một vấn đề, giảm thời gian phản hồi tổng thể
- Cải thiện chất lượng giải pháp – Mỗi agent tập trung vào chuyên môn của mình, dẫn đến các phản hồi chính xác và phù hợp hơn

Thực tiễn tốt nhất: Chọn các mô hình phù hợp cho mỗi agent chuyên biệt, khớp khả năng với yêu cầu tác vụ trong khi tối ưu hóa cho chi phí. Dựa trên độ phức tạp của tác vụ, bạn có thể chọn mô hình chi phí thấp hoặc mô hình chi phí cao để tối ưu hóa chi phí. Sử dụng các hàm [AWS Lambda](https://aws.amazon.com/lambda) chỉ lấy dữ liệu thiết yếu để giảm chi phí không cần thiết trong việc thực thi Lambda. Điều phối hệ thống của bạn với một agent supervisor nhẹ xử lý hiệu quả việc phối hợp mà không tiêu thụ tài nguyên premium.

## Chọn throughput mong muốn tùy thuộc vào việc sử dụng
Amazon Bedrock cung cấp hai tùy chọn throughput riêng biệt, mỗi cái được thiết kế cho các pattern sử dụng và yêu cầu khác nhau:

- Chế độ On-Demand – Cung cấp cách tiếp cận pay-as-you-go không có cam kết trước, làm cho nó lý tưởng cho các bằng chứng khái niệm (POC) giai đoạn đầu trong môi trường phát triển và kiểm thử, các ứng dụng với lưu lượng không thể đoán trước hoặc theo mùa hoặc không thường xuyên với sự biến đổi đáng kể.
  Với giá On-Demand, bạn được tính phí dựa trên việc sử dụng thực tế:

  - Các mô hình tạo văn bản – Trả tiền cho mỗi token đầu vào được xử lý và token đầu ra được tạo
  - Các mô hình embedding – Trả tiền cho mỗi token đầu vào được xử lý
  - Các mô hình tạo hình ảnh – Trả tiền cho mỗi hình ảnh được tạo
- Chế độ Provisioned Throughput – Bằng cách sử dụng Provisioned Throughput, bạn có thể mua các đơn vị mô hình chuyên dụng cho các FM cụ thể để có mức throughput cao hơn cho một mô hình với chi phí cố định. Điều này làm cho Provisioned Throughput phù hợp cho workload sản xuất yêu cầu hiệu suất có thể đoán trước mà không bị throttling. Nếu bạn tùy chỉnh một mô hình, bạn phải mua Provisioned Throughput để có thể sử dụng nó.

Mỗi đơn vị mô hình cung cấp một khả năng throughput được định nghĩa được đo bằng số lượng token tối đa được xử lý mỗi phút. Provisioned Throughput được tính phí theo giờ với các tùy chọn cam kết 1 tháng hoặc 6 tháng, với cam kết dài hơn cung cấp giảm giá lớn hơn.

Thực tiễn tốt nhất: Nếu bạn đang làm việc trên POC hoặc trên trường hợp sử dụng có workload không thường xuyên sử dụng một trong những FM cơ bản từ Amazon Bedrock, hãy sử dụng chế độ On-Demand để tận dụng lợi ích của giá pay-as-you-go. Tuy nhiên, nếu bạn đang làm việc trên workload trạng thái ổn định nơi throttling phải được tránh, hoặc nếu bạn đang sử dụng các mô hình tùy chỉnh, bạn nên chọn provisioned throughput phù hợp với workload của bạn. Tính toán yêu cầu xử lý token của bạn một cách cẩn thận để tránh over-provisioning.

## Sử dụng suy luận batch
Với chế độ batch, bạn có thể có được các dự đoán quy mô lớn đồng thời bằng cách cung cấp một tập hợp các prompt như một tệp đầu vào duy nhất và nhận phản hồi như một tệp đầu ra duy nhất. Các phản hồi được xử lý và lưu trữ trong bucket [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3) của bạn để bạn có thể truy cập chúng sau này. Amazon Bedrock cung cấp các FM được chọn từ các nhà cung cấp AI hàng đầu như Anthropic, Meta, Mistral AI, và Amazon cho suy luận batch với giá thấp hơn 50% so với giá suy luận On-Demand. Tham khảo [Các AWS Region và mô hình được hỗ trợ cho suy luận batch](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference-supported.html) để biết thêm chi tiết. Cách tiếp cận này lý tưởng cho các workload không thời gian thực nơi bạn cần xử lý khối lượng lớn nội dung một cách hiệu quả.

Thực tiễn tốt nhất: Xác định các workload trong ứng dụng của bạn không yêu cầu phản hồi thời gian thực và di chuyển chúng sang xử lý batch. Ví dụ, thay vì tạo mô tả sản phẩm theo yêu cầu khi người dùng xem chúng, hãy pre-generate mô tả cho các sản phẩm mới trong một công việc batch hàng đêm và lưu trữ kết quả. Cách tiếp cận này có thể giảm đáng kể chi phí FM của bạn trong khi duy trì cùng chất lượng đầu ra.

## Kết luận
Khi các tổ chức ngày càng áp dụng Amazon Bedrock cho các ứng dụng AI tạo sinh của họ, việc triển khai các chiến lược tối ưu hóa chi phí hiệu quả trở nên quan trọng để duy trì hiệu quả tài chính. Chìa khóa cho tối ưu hóa chi phí thành công nằm ở việc có một cách tiếp cận có hệ thống. Đó là, bắt đầu với các tối ưu hóa cơ bản như lựa chọn mô hình phù hợp và kỹ thuật prompt, sau đó tiệm tiến triển khai các kỹ thuật tiên tiến hơn như caching và xử lý batch khi các trường hợp sử dụng của bạn trưởng thành. Giám sát thường xuyên các pattern chi phí và sử dụng, kết hợp với tối ưu hóa liên tục các chiến lược này, sẽ giúp đảm bảo rằng các sáng kiến AI tạo sinh của bạn vẫn hiệu quả và bền vững về mặt kinh tế.

Hãy nhớ rằng tối ưu hóa chi phí là một quá trình đang diễn ra nên phát triển theo nhu cầu và pattern sử dụng của ứng dụng bạn, làm cho việc thường xuyên xem xét và điều chỉnh việc triển khai các chiến lược này trở nên thiết yếu.

Để biết thêm thông tin về giá cả Amazon Bedrock và các chiến lược tối ưu hóa chi phí được thảo luận trong bài viết này, tham khảo:

- [Giá cả Amazon Bedrock](https://aws.amazon.com/bedrock/pricing/)
- [Amazon Bedrock Model Distillation](https://aws.amazon.com/bedrock/model-distillation/)
- [Amazon Bedrock Intelligent Prompt Routing](https://aws.amazon.com/bedrock/intelligent-prompt-routing/)
- [Bộ nhớ đệm prompt Amazon Bedrock](https://aws.amazon.com/bedrock/prompt-caching/)
- [Xử lý nhiều prompt với suy luận batch](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference.html)
- [Giám sát hiệu suất của Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/monitoring.html)