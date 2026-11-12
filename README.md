# Phân tích và dự báo điện năng hằng ngày

Toàn bộ quy trình nằm trong **notebooks/electricity_forecasting.ipynb**. Notebook đọc trực tiếp `load_forecasting_dataset_corrected.csv`; không cần module Python nội bộ hoặc file dữ liệu đã xử lý sẵn. Mỗi bước có markdown giải thích trước ô code.

## Nội dung notebook

1. Thiết lập thư viện và đường dẫn.
2. Xác minh dữ liệu 15 phút, tổng hợp điện năng ngày và ghi `data/processed/daily_energy.csv`.
3. Phân tích xu hướng, phân phối, mùa vụ, ACF/PACF và thời tiết.
4. Chia tập theo thời gian, tạo đặc trưng và kiểm chứng không sử dụng tương lai.
5. Định nghĩa cấu hình mô hình, huấn luyện, dự báo, backtest và chỉ số sai số ngay trong notebook.
6. Chạy validation và khóa cấu hình.
7. Đánh giá test, lưu MAE/RMSE, dự báo và biểu đồ phần dư.
8. Đánh giá từng chân trời và dự báo 7 ngày tiếp theo.
9. Tổng hợp kết luận và lưu phiên bản môi trường.

## Cài đặt và chạy

Dùng Python 3.10 trở lên trong môi trường có thể tải thư viện:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --prefix .venv --name python3
python -m jupyterlab
```

Mở `notebooks/electricity_forecasting.ipynb`, chọn kernel của môi trường vừa cài, rồi **Restart Kernel → Run All**. Chạy lần lượt từ trên xuống; các bước sau sử dụng biến được tạo ở bước trước. Có thể thực thi tự động từ thư mục gốc:

```bash
jupyter nbconvert --execute --to notebook --inplace --ExecutePreprocessor.timeout=1800 notebooks/electricity_forecasting.ipynb
```

## Dữ liệu và đầu ra

- CSV gốc là đầu vào duy nhất; không bị chỉnh sửa.
- `data/processed/daily_energy.csv`: dữ liệu ngày do notebook tạo lại.
- `outputs/figures/`: biểu đồ PNG.
- `outputs/tables/`: kiểm tra chất lượng, bảng sai số, cảnh báo và kết luận.
- `outputs/predictions/`: dự báo test, dự báo nhiều bước và 7 ngày tương lai.
- `outputs/models/`: cấu hình chọn và mô hình cuối.
- `outputs/environment.json`: phiên bản môi trường chạy.

Các lần chạy ghi đè đầu ra tương ứng. Dữ liệu công suất được giả định là trung bình mỗi 15 phút; điện năng ngày = tổng 96 giá trị × 0,25 giờ. Giả định chưa được xác minh từ tài liệu nguồn.

Train 2020–2023; validation 2024; test 01–05/2025. Chọn theo MAE validation, dùng RMSE phá hòa. Mô hình gồm Naive, Seasonal Naive, ETS, SARIMA, Random Forest và HistGradientBoosting. Dự báo cuối là 01–07/06/2025, tính từ ngày kết thúc dữ liệu. Quy trình và cấu trúc outputs được giữ như trước khi gom notebook.

## Trạng thái kiểm chứng

Đã kiểm tra cú pháp các ô code và thứ tự định nghĩa, xác nhận mỗi ô code có markdown giải thích. Dữ liệu ngày đã được tổng hợp trong bước triển khai trước. Notebook mới chưa chạy xuyên suốt trong môi trường này vì thiếu thư viện phân tích; chưa có kết quả huấn luyện được xác nhận. Cần Run All sau khi cài đủ requirements để kiểm chứng kết quả thực thi.
