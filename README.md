# level_2_milestone_2_notes_aggregation
# MongoDB Aggregation - বাংলা নোট

MongoDB তে **Aggregation** হল ডেটা প্রক্রিয়া করার একটি শক্তিশালী পদ্ধতি, যা ডেটা ফিল্টার, গ্রুপ, সাজানো, এবং পরিসংখ্যান তৈরি করতে ব্যবহৃত হয়। নিচে বিভিন্ন **Aggregation** অপারেটরের উদ্দেশ্য, **Syntax** এবং **Example** দেওয়া হলো।

---

### ১. **$match**
**উদ্দেশ্য**: নির্দিষ্ট শর্ত অনুসারে ডকুমেন্ট ফিল্টার করা।

**Syntax**:
```
{ $match: { <condition> } }
```
**Example**
```
db.users.aggregate([
  { $match: { age: { $gt: 30 } } }
])
```
---

### 2. **$project**
**উদ্দেশ্য**: কোন ফিল্ডগুলো ইনক্লুড বা এক্সক্লুড করতে হবে তা নির্ধারণ করা।

**Syntax**:
```
{ $project: { <field1>: 1, <field2>: 0, ... } }
```
**Example**
```
db.users.aggregate([
  { $project: { name: 1, age: 1, _id: 0 } }
])
```
---

### 3. **$sort**
**উদ্দেশ্য**: এক বা একাধিক ফিল্ডের ওপর ডেটা সাজানো।

**Syntax**:
```
{ $sort: { <field1>: 1, <field2>: -1, ... } }
```
**Example**
```
db.users.aggregate([
  { $sort: { age: 1 } }
])
```
---

### 4. **$limit**
**উদ্দেশ্য**:  ডকুমেন্টের সংখ্যা সীমিত করা।

**Syntax**:
```
{ $limit: <number> }
```
**Example**
```
db.users.aggregate([
  { $limit: 5 }
])
```
---

### 5. **$out**
**উদ্দেশ্য**:  অ্যাগ্রিগেশন রেজাল্ট অন্য একটি কালেকশনে সংরক্ষণ করা।

**Syntax**:
```
{ $out: "<collection_name>" }
```
**Example**
```
db.users.aggregate([
  { $match: { age: { $gt: 30 } } },
  { $out: "older_users" }
])
```
---

### 6. **$merge**
**উদ্দেশ্য**:  অ্যাগ্রিগেশন রেজাল্ট বিদ্যমান কালেকশনে সংরক্ষণ করা, একই সাথে ডেটা আপডেট বা ইনসার্ট করা।

**Syntax**:
```
{ $merge: { into: "<collection_name>" } }
```
**Example**
```
db.users.aggregate([
  { $match: { age: { $gt: 30 } } },
  { $merge: { into: "updated_users" } }
])
```
---

### 7. **$group**
**উদ্দেশ্য**:   একটি নির্দিষ্ট ফিল্ড অনুসারে ডকুমেন্ট গ্রুপ করা এবং অ্যাগ্রিগেশন (যেমন: sum, avg) করা।

**Syntax**:
```
{ $group: { _id: "<field_name>", <aggregation_field>: { $aggregation_operator: <field> } } }

```
**Example**
```
db.users.aggregate([
  { $group: { _id: "$address.country", totalSalary: { $sum: "$salary" } } }
])
```
---

### 8. **$push**
**উদ্দেশ্য**:   গ্রুপ স্টেজে একাধিক ডকুমেন্টের মান একত্রিত করে একটি অ্যারে তৈরি করা।

**Syntax**:
```
{ $push: "<field>" }

```
**Example**
```
db.users.aggregate([
  { $group: { _id: "$address.country", users: { $push: "$name" } } }
])

```
---

### 9. **$count**
**উদ্দেশ্য**:   একটি নির্দিষ্ট ফিল্ড অনুযায়ী ডকুমেন্টের সংখ্যা গণনা করা।।

**Syntax**:
```
{ $count: "<count_field>" }

```
**Example**
```
db.users.aggregate([
  { $count: "total_users" }
])
```
---

### 10. **$sum | $max | $min | $avg**
**উদ্দেশ্য**:   বিভিন্ন অ্যাগ্রিগেশন অপারেটর যেমন, একটি ফিল্ডের মোট (sum), সর্বোচ্চ (max), সর্বনিম্ন (min), গড় (avg) মান বের করা।

**Example**
```
db.users.aggregate([
  { $group: { _id: "$address.country", 
              totalSalary: { $sum: "$salary" }, 
              maxSalary: { $max: "$salary" },
              minSalary: { $min: "$salary" },
              avgSalary: { $avg: "$salary" } 
            } }
])
```
---

### 11. **$unwind**
**উদ্দেশ্য**:   একটি অ্যারে ফিল্ডকে একাধিক ডকুমেন্টে ভেঙে ফেলা।

**Syntax**:
```
{ $unwind: "<array_field>" }

```
**Example**
```
db.orders.aggregate([
  { $unwind: "$items" }
])

```
---

### 12. **$bucket**
**উদ্দেশ্য**:   ডেটাকে নির্দিষ্ট রেঞ্জে বা বাচেটে ভাগ করা এবং প্রতিটি বাচেটের উপর কিছু পরিসংখ্যান তৈরি করা।

**Syntax**:
```
{ 
  $bucket: { 
    groupBy: "<field_name>", 
    boundaries: [<range_1>, <range_2>, ..., <range_n>],
    default: "<default_category>", 
    output: { <aggregation_field>: { $aggregation_operator: <field> } }
  }
}


```
**Example**
```
db.users.aggregate([
  { 
    $bucket: { 
      groupBy: "$age", 
      boundaries: [20, 30, 40, 50], 
      default: "Other", 
      output: { count: { $sum: 1 } }
    } 
  }
])

```
---

### 13. **$bucket**
**উদ্দেশ্য**:   ডেটাকে নির্দিষ্ট রেঞ্জে বা বাচেটে ভাগ করা এবং প্রতিটি বাচেটের উপর কিছু পরিসংখ্যান তৈরি করা।

**Syntax**:
```
{ 
  $lookup: { 
    from: "<collection_name>", 
    localField: "<local_field>", 
    foreignField: "<foreign_field>", 
    as: "<new_field>" 
  }
}


```
**Example**
```
db.orders.aggregate([
  { 
    $lookup: { 
      from: "customers", 
      localField: "customer_id", 
      foreignField: "_id", 
      as: "customer_details" 
    }
  }
])

```
---

### 13. **$facet**
**উদ্দেশ্য**:   একাধিক পাইপলাইন একসাথে চালানোর জন্য ব্যবহৃত হয়।

**Syntax**:
```
{ 
  $facet: {
    <pipeline_name>: [ <stages> ]
  }
}

```
**Example**
```
db.users.aggregate([
  { 
    $facet: {
      "age_groups": [
        { $match: { age: { $gte: 30 } } },
        { $count: "adults" }
      ],
      "salary_groups": [
        { $match: { salary: { $gte: 50000 } } },
        { $count: "high_salary" }
      ]
    }
  }
])

```
---
