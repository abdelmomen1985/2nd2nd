```bash
FILE="{{file_path:absolute}}"

# Remove any unintended backslashes
FILE=$(echo "$FILE" | sed 's/\\//g')

# Ensure the file has a trailing newline and extract URLs
sed -nE 's|(https?://[^[:space:]]+)|\1|p' "$FILE" | while IFS= read -r url || [ -n "$url" ]; do
  url="${url#-}"
  xdg-open $url
done
```
### **Explanation & Summary of the Script**

This Bash script extracts URLs from a file and opens them using `xdg-open`, ensuring proper handling of file paths, backslashes, and edge cases like missing trailing newlines.

---

### **Step-by-Step Breakdown:**

1. **Get the file path from Obsidian**
    
    ```bash
    FILE="{{file_path:absolute}}"
    ```
    
    - `{{file_path:absolute}}` is an Obsidian placeholder for the full file path.
2. **Fix Backslashes in the File Path**
    
    ```bash
    FILE=$(echo "$FILE" | sed 's/\\//g')
    ```
    
    - Converts backslashes (`\`) to forward slashes (`/`), ensuring compatibility with Unix-based systems.
3. **Extract URLs from the File**
    
    ```bash
    sed -nE 's|(https?://[^[:space:]]+)|\1|p' "$FILE"
    ```
    
    - **`sed -nE`**: Uses extended regex (`-E`) and suppresses default output (`-n`).
    - **Regex Explanation:**
        - Matches `http://` or `https://` followed by any non-space characters.
        - Prints (`p`) only the matched URLs.
4. **Loop Through Each Extracted URL & Open in Browser**
    
    ```bash
    while IFS= read -r url || [ -n "$url" ]; do
      url="${url#-}"
      xdg-open $url
    done
    ```
    
    - **`IFS= read -r url || [ -n "$url" ]`**
        - Ensures the **last URL is processed** even if the file **does not end with a newline**.
    - **`url="${url#-}"`**
        - Removes a leading `-` to prevent errors when passing the URL to `xdg-open`.
    - **`xdg-open $url`**
        - Opens each extracted URL in the default browser.

---

### **Summary:**

This script: ✔ Fixes file path issues (backslashes).  
✔ Extracts all URLs from an Obsidian file.  
✔ Ensures no URLs start with `-` (avoiding errors).  
✔ Ensures the **last URL is not skipped**.  
✔ Opens each URL using `xdg-open`.

It's a **robust and efficient** way to open multiple URLs stored in an Obsidian note! 🚀