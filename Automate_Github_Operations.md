You can automate the GitHub operations and include the current date dynamically in your commit message using the date command. Here’s a clean and complete version of your script:

```bash
#!/bin/bash

echo -e "\nCommencing GitHub commit..."

git add -A

# Format: "New database export - 2025-04-11"
git commit -m "New database export - $(date +'%Y-%m-%d')"

git push origin master

echo "Successful GitHub operations!"
```
* The -e flag enables interpretation of escape characters like \n.

## 🔧 Customizing the date format

You can change $(date +'%Y-%m-%d') to any format you prefer:

* $(date +'%d-%m-%Y') → 11-04-2025
* $(date +'%A, %B %d, %Y') → Friday, April 11, 2025
