

## 📝 Challenge Description
The goal was to fragment a single `data.csv` file into exactly 10 smaller parts (`data-00.csv` to `data-09.csv`). A key requirement was that each resulting fragment must retain the original CSV header, and no fragment should exceed 32KB.

---

## 🔍 Investigation & Strategy
The standard `split` utility in Linux is excellent for dividing files by size or line count, but it doesn't natively prepend a header to each chunk. 

### Implementation Logic:
1. **Header Extraction:** Capture the first line of the source file.
2. **Body Splitting:** Stream the file content (starting from line 2) into the `split` command to create 10 chunks.
3. **Reconstruction:** Loop through the generated chunks, prepending the saved header to each, and saving them with the required naming convention.

---

## 🛠️ The Fix

```bash
# 1. Capture Header
header=$(head -n 1 /home/admin/data.csv)
```

# 2. Split the body (excluding header) into 10 parts
```
tail -n +2 /home/admin/data.csv | split -n 10 -d --numeric-suffixes=00 - temp_part-
```

# 3. Prepend header to each part and rename
```
for i in {00..09}; do
    echo "$header" > /home/admin/data-$i.csv
    cat temp_part-$i >> /home/admin/data-$i.csv
    rm temp_part-$i
```
done
