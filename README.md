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

### 4. **$sort**
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
