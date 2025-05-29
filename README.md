### To (re)create an EFI Partition Using `gdisk` in macOS

### Steps:
1. **Identify disk**  
   ```bash
   diskutil list  # Note target disk (e.g., /dev/disk0)
   ```

2. **Launch `gdisk`**  
   ```bash
   sudo gdisk /dev/disk0  # Replace `disk0` with your disk
   ```

3. **Delete existing EFI partition**  
   In `gdisk`:  
   ```bash
   p             # List partitions
   d             # Delete
   [Partition #] # Enter EFI partition number (e.g., 1)
   ```

4. **Create new partition**  
   In `gdisk`:  
   ```bash
   n                          # New partition
   [Enter]                    # Default partition number
   [Enter]                    # Default first sector
   +200M                      # Partition size
   t                          # Change type
   [New partition #]          # Partition number
   EF00                       # EFI type code
   ```

5. **Write changes**  
   In `gdisk`:  
   ```bash
   w  # Write table
   y  # Confirm
   ```

6. **Format partition**  
   ```bash
   sudo mkfs.fat -F32 /dev/disk0s1  # Replace with new partition ID
   ```

---

### Minimal Workflow Example:
```bash
diskutil list
sudo gdisk /dev/disk0
p
d
[EFI-partition-#]
n
[Enter]
[Enter]
+200M
t
[new-partition-#]
EF00
w
y
sudo mkfs.fat -F32 /dev/disk0s1
```
