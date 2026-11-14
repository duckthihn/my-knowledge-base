Nguồn: https://www.youtube.com/watch?v=a0NdOrVxF3s&t=603s (youtube)

# Tại sao cần optimize
- improve throughput (scan-time)
- reduce I/O () <- data lake không được quản lý tốt sẽ dẫn đến data swamp (lộn xộn, khó sử dụng)
- reduce small files problems (compact)
- reduce query latency
- reduce storage cost

iceberg -> cấp độ file -> dựa vào file metadata.json
![[Pasted image 20261114134748.png|481]]
![[Pasted image 20261114134958.png]]

hive -> cấp độ Folder/Direction/Partition
![[Pasted image 20261114134621.png|264]]