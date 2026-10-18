Forensics in the photo 
all bytes header png  reversed

# Challenge Q: PNG with reversed bytes

**Problem:** `a.bin` was a PNG, but `file` said `data`. In hexedit the signature was scrambled (89 was in the 4th position).
**Idea:** Every 4-byte chunk was reversed. Reverse them back, then binwalk found a second PNG appended after the first one's IEND.
**Script:**
```python
data = open("a.bin", "rb").read()
opt = b"".join(data[i:i+4][::-1] for i in range(0, len(data), 4))
open("fixed.png", "wb").write(opt)
```
**Commands:** `binwalk -e fixed.png` -> second PNG at offset 60048 -> `opt[60048:]` (or `dd skip=60048 ibs=1`)
**Lesson:** If `file` says `data`, check the magic bytes. Binwalk finds files glued together.
