Nguồn: https://www.youtube.com/watch?v=a0NdOrVxF3s&t=603s (youtube)

# Tại sao cần optimize
- improve throughput (scan-time)
- reduce I/O () <- data lake không được quản lý tốt sẽ dẫn đến data swamp (lộn xộn, khó sử dụng)
- reduce small files problems (compact)
- reduce query latency
- reduce storage cost
- đảm bảo tính atomic metadata commit (không phá commit/writer của iceberg)
- xử lý concurrency và multi writer và rollback khi lỗi

iceberg -> cấp độ file -> dựa vào file metadata.json
![[Pasted image 20261114134748.png|481]]
![[Pasted image 20261114134958.png]]

hive -> cấp độ Folder/Direction/Partition
![[Pasted image 20261114134621.png|264]]

# Khi nào cần optimize data lakehouse
- quá nhiều file nhỏ (small files)
- fragmentation (file/row-group phân mảnh trong partition)
- dư thừa file
- phân vùng không đều (data/partition skew)
- tỉ lệ delete/overwrite cao (nhiều tombstone issue/delete files)
- chỉ số format-specific (ví dụ: manifest count trong Iceberg  / delta file trong Hudi)


core
- theo dõi metadata của các bảng
- phát hiện ra bảng nào cần optimize
- tự sinh ra các optimize task (automatically generate optimize task)
- gửi các task tới optimizer/executor
- theo dõi, nhận kết quả, cập nhật status của bảng
