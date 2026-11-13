Official document: 

Spark Fundamentals: Day 1 Notes

Spark relearn

# References

https://substack.com/home/post/p-207997209

https://substack.com/home/post/p-189222561

https://www.youtube.com/watch?v=aKYDDn4wU8c

# Checklist

Day 1

- [x]  Mental model + DAG plan / (Job → Stage → Task)
- [x]  Narrow vs. Wide transformation
- [x]  Partitioning
- [x]  Shuffle
- [x]  Spark Cluster: Driver Master Worker

# Mental model

!image.png

```python
df = spark.read.parquet("s3://bucket/orders/")      # 2 GB

result = (df
    .filter(df.status == "completed")                # lọc
    .select("country", "amount")                     # chọn cột
    .groupBy("country").sum("amount")                # shuffle #1
    .orderBy("sum(amount)", ascending=False))        # shuffle #2

result.write.parquet("s3://bucket/revenue_by_country/")   # ACTION → tạo job
```

# DAG plan

!image.png

Mỗi action tạo ra 1 job. Spark dựng DAG của job và chia thành các stage tại những điểm cần shuffle (**wide transformation** như `groupBy`, `join`), còn các **narrow transformation** như `filter`, `select` được gộp vào cùng stage. Trong mỗi stage, mỗi partition được xử lý bởi 1 task.

> Action → Job → chia Stage tại mỗi shuffle (wide transformation) → mỗi Stage gồm các Task, 1 task cho 1 partition.
> 

→ Those “boundaries” matter. They’re where shuffles happen, where stages split, where time explodes.

## Narrow vs Wide transformation

!image.png

(demo ở section Shuffle)

# Partitioning

***Spark never processes the DataFrame as one giant object. It always breaks it into multiple input partitions***

A partition is just **one unit of work.** Each partition becomes a task. Tasks run on executors.

→ partitioning quyết định

- how much parallelism you get
- how much data each task handles
- how painful the shuffle will be

Input partitions vs. shuffle partitions: 

- **Input partitioning** — how data enters Spark. Files split into partitions; the source determines the starting layout.
- **Shuffle partitioning** — how Spark repartitions during wide ops (shuffle). Controlled by settings and APIs like `repartition` and `coalesce`. This often decides your runtime.

```jsx
2,048 MiB / 128 MiB = 16 input partitions # if max size is 128
2,048 MiB / 256 MiB = 8 input partitions # if max size is 256

3,000 / 256 = 11.71875

=> 12 input partitions

Partitions 1–11: ~256 MiB
Partition 12:     ~184 MiB
```

There will always be trade of between high vs low number of input partitions

# **The “too few vs too many” rule of thumb**

- Too few partitions → each task is huge → slow tasks, spills, poor parallelism
- Too many partitions → overhead explodes → scheduling cost, tiny tasks, too much shuffle metadata

!image.png

`repartition(n, col)` — forces a shuffle. Useful when you want to align data by a key before a heavy operation. Good for joins and groupBys when your current layout is terrible.

`coalesce(n)` — reduces partitions *without* a full shuffle (best-effort). Useful after filters, when the dataset shrank and you don’t want 50,000 tiny output files.

![](https://substackcdn.com/image/fetch/$s_!YRcL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faff4088e-2b24-4a0d-85ae-b897557c13c7_2026x832.png)

A practical habit — ask this before and after every major step:

1. How many partitions do I have right now?
2. Are they evenly sized?
3. Did I just create a layout that makes the next operation expensive?

If you can answer those 3 questions, you’re already ahead of 90% of Spark learners.

(will test later)

# Shuffle

spark_input_to_shuffle_partition_walkthrough.html

Shuffle is when Spark must move data across the network because the next step needs data regrouped differently.

The classic triggers:

- `groupBy(key)` — all rows with the same key must land together
- joins on keys — matching rows may be sitting on different executors

**What shuffle actually does:** imagine 10 workers, each holding a chunk of data, and you call `groupBy(user_id)`. Spark must ensure every `user_id` ends up in exactly one place so it can aggregate correctly.

!image.png

So each worker splits its data into buckets, writes those buckets out (often to disk), sends them across the network, the other side reads them — and only *then* can the aggregation happen.

Shuffle is expensive because it hits all the painful things at once: network I/O, serialization, disk spill, task overhead, and skew.

## **The 3 symptoms of shuffle problems**

If a job is slow and you see any of these, think shuffle first:

1. Stages take forever right after a `join` or `groupBy`
2. Tasks show “spill” (memory → disk) -  nếu partition quá lớn, execution memory không đủ và Spark phải đổ tạm ra đĩa.
3. One or two tasks are 50x slower than the rest (data skew) - nếu một key chiếm quá nhiều dữ liệu (ví dụ một country chiếm 80%), một task phải xử lý gần hết và cả stage phải chờ nó.

!image.png

### Cách giảm chi phí shuffle

- `filter` và `select` **trước** `groupBy`/`join` để giảm lượng dữ liệu phải shuffle (đúng như Stage 1 trong hình).
- Dùng **broadcast join** khi một bảng đủ nhỏ, để bỏ hẳn shuffle của join.
- Tận dụng **partial aggregate** (Spark tự làm) để gửi ít dữ liệu hơn.
- Chỉnh `spark.sql.shuffle.partitions` hoặc bật AQE để số shuffle partition hợp lý.

(will test later)

# The workers

The worker are just a bunch of resources that’s **INDEPENDENT** from the spark cluster itself.

Spark cluster can utilize some (or all of these resources) but they exist independently from the cluster itself. For example: Imagine two EC2 instances, each with 8 vCPUs and 16 GB of RAM. The instances exist before any Spark application starts and remain running after it finishes. They may also run other workloads.

```jsx
+----------------------+  +----------------------+
| EC2 worker 1         |  | EC2 worker 2         |
| Available resources  |  | Available resources  |
+----------------------+  +----------------------+
```

***Do not confuse workers with executors***

The EC2 instances are the underlying worker machines. Executors are temporary processes created for a particular Spark application.

# Spark Submit

`spark-submit` sends your application code and configuration to the cluster. It is the command that **starts a Spark application**, not the component that processes the data.

```powershell
spark-submit \
  --master spark://master-host:7077 \
  app.py
```

# **The Spark Cluster (Spark Master, Driver, Executor)**

A Spark cluster usually contain of 3 components:

- Spark Master: resource manager
- Driver: brain of one Spark application
- Executor: do the work

!image.png

```python
                 ┌──────────────────────┐
  spark-submit → │  Master              │
                 │  (cluster manager)   │
                 └─────┬──────────┬─────┘
          ③ cấp tài nguyên        │
                 ▼                ▼
        ┌──────────────┐  ┌──────────────┐
        │ Worker 1     │  │ Worker 2     │
        │ ┌──────────┐ │  │ ┌──────────┐ │
        │ │Executor A│ │  │ │Executor B│ │
        │ └────▲─────┘ │  │ └────▲─────┘ │
        └──────┼───────┘  └──────┼───────┘
               │ ⑤ gửi task, nhận kết quả
               └───────┬─────────┘
                  ┌────┴─────┐
                  │  Driver  │ ① chạy code, dựng DAG
                  └──────────┘ ② xin tài nguyên từ Master
```

### Luồng hoạt động

1. `spark-submit` khởi động application và driver bắt đầu chạy code của bạn.
2. Driver xin tài nguyên (số executor, core, RAM) từ master.
3. Master chọn các worker còn trống và **khởi chạy executor** trên đó.
4. Executor **đăng ký ngược lại với driver** để báo “tôi sẵn sàng”.
5. Khi gặp action, driver tạo job → stage → task, rồi **gửi task thẳng tới executor**.
6. Executor xử lý partition và trả kết quả (hoặc ghi ra storage).
7. Application kết thúc, executor bị hủy, worker vẫn còn đó.

# **A beginner-friendly debugging checklist (save this)**

# **How these 5 ideas connect (this is the real unlock)**

Take a very common pipeline: read events → filter → join user dimension → groupBy user_id → write output.

![](https://substackcdn.com/image/fetch/$s_!YeQo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0882fa2e-c6c1-40bd-bf15-7a2999b266d0_1788x1004.png)

Here’s what’s likely happening:

- Read/filter/select are narrow → cheap → same stage
- The join might trigger a shuffle unless broadcast is possible
- The groupBy *definitely* triggers a shuffle
- The shuffle creates new partitions
- Memory pressure peaks during the join/groupBy
- If memory is tight → spills → huge slowdown

So instead of “Spark is slow,” you now say:

- “This groupBy causes a shuffle and my shuffle partitions are too large.”
- “This join is not broadcast; it’s doing a sort-merge with an Exchange.”
- “I’m spilling during aggregation; I should reduce data pre-shuffle or fix skew.”
- “My partitions are unbalanced; one key is dominating.”

That’s the difference between a Spark *user* and a Spark *engineer*.

# **A beginner-friendly debugging checklist (save this)**

When a Spark job is slow, do this in order:

![](https://substackcdn.com/image/fetch/$s_!nE6d!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9e7f5e9a-79e2-4321-9a4c-601e0482b39b_816x627.svg)

**Step 1: Identify the first expensive stage.** Which stage takes the most time? What operation happens right before it — join, groupBy, distinct, orderBy? If it’s one of those, assume shuffle until proven otherwise.

**Step 2: Check for skew.** Are a few tasks much slower than the rest? Do they process way more input? If yes, you have skew — and adding executors won’t fix it. You need a skew strategy.

**Step 3: Check partition counts and sizes.** Too few → giant tasks → spills. Too many → overhead.

**Step 4: Check the join strategy.** Is it broadcasting? Is it sorting + exchanging?

**Step 5: Check spill and memory symptoms.** If spilling is high: reduce data before the shuffle (filter/select early), pre-aggregate, increase parallelism, stop caching junk, fix skew.

Do this a few times and Spark becomes way less mysterious.

# **The best way to learn Spark in 2026 (without drowning)**

If you’re new, here’s a learning path that actually works:

1. **Write DataFrame code.** Yes, use the API.
2. **After every major transformation, ask:** is this narrow or wide? Did I just cause a shuffle? What happened to my partitions?
3. **Peek at the plan.** Look for `Exchange`, `BroadcastHashJoin`, `SortMergeJoin`.
4. **Watch the Spark UI like a story:** stages = chapters, shuffles = plot twists, skew = the villain.

![](https://substackcdn.com/image/fetch/$s_!o51Y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb75a04a9-a935-4f0c-9062-fe7c3e77e40c_2260x820.png)

That’s it. That’s the game.

# **Small challenge for you (do this today)**

Pick any dataset you have (even 1 GB is fine) and build this: read → filter to the last 30 days → join with a small dimension table → groupBy a key → write the output.

Now, **before running**, predict:

- where the shuffle will happen
- whether broadcast is possible
- whether the output will create many small files
- where memory might spike

![](https://substackcdn.com/image/fetch/$s_!fF0x!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f0c24ad-8024-4c1b-be42-e451ee151c35_2140x994.png)

Then run it and compare.

This one exercise will teach you more than 20 random Spark tutorials.