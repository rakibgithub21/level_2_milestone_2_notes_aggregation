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
