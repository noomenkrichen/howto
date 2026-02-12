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

## Colored Text in Linux Terminal

You can echo colored text in the Linux terminal using ANSI escape codes.

Here’s a basic example:
```bash
echo -e "\e[31mThis text is red\e[0m"
```
* -e tells echo to interpret backslash escapes.
* \e[31m starts red color.
* \e[0m resets the color back to normal.

Some common color codes:
| Color	| Code |
|-------|------|
| Black	| 30 |
| Red	|	31 |
| Green	|	32 |
| Yellow	|	33 |
| Blue	|	34 |
| Magenta	|	35 |
| Cyan	|	36 |
| White	|	37 |

### Example with different colors:
```bash
echo -e "\e[32mSuccess!\e[0m"
echo -e "\e[33mWarning!\e[0m"
echo -e "\e[31mError!\e[0m"
```
### If you want bold text too, you can use 1:
```bash
echo -e "\e[1;34mBold Blue Text\e[0m"
```

## Fedora
In Fedora, the echo command built into Bash sometimes behaves differently: echo -e is not guaranteed to work the same way.
Fedora recommends using printf instead.

✅ Try using printf like this instead:
```bash
printf "\e[31mThis text is red\e[0m\n"
```

* printf always interprets escape sequences like \e, \n, etc.
* It's more portable and reliable across all Linux distros (Ubuntu, Fedora, Arch, etc.).

### Same examples using printf:
```bash
printf "\e[32mSuccess!\e[0m\n"
printf "\e[33mWarning!\e[0m\n"
printf "\e[31mError!\e[0m\n"
```

✅ Instead of relying on echo -e, use printf — it's portable across all shells.

Change your .sh file line from:
```bash
echo -e "\e[35m========================================================="
```
to:
```bash
printf "\e[35m=========================================================\n"
```

* printf works reliably in Dash, Bash, Zsh, Ksh, etc.
* \n at the end means "newline" (just like pressing Enter).

## Bonus Tip: (even better)

At the top of your .sh file, force Bash if you want Bash-specific behavior:
```bash
#!/bin/bash
```
instead of:
```bash
#!/bin/sh
```
But if you want maximum compatibility, just stick with printf.

## Troubleshooting
If printf is not working in your script either, it's likely related to how the shell is interpreting escape sequences or how it's handling the script's environment.

Let’s troubleshoot this:

1. Ensure the script uses Bash:
Make sure you are invoking the script with Bash. If you want to ensure the script runs with Bash, set the shebang at the top of your .sh file:
```bash
#!/bin/bash
```

2. Check for escape sequence support:
Escape sequences like \e[35m should work in most terminals. However, if for any reason it's not, you can try \033 instead of \e. These are two ways to write the same thing, but some terminals or shells may handle them differently:
```bash
printf "\033[35m=========================================================\n"
```

3. Make sure the script is executable:
Ensure the shell script is executable by running:
```bash
chmod +x yourscript.sh
```

4. Run the script directly with Bash:
Even if you have the correct shebang, running the script like this ensures it's executed with Bash:
```bash
bash yourscript.sh
```

5. Run the script in an interactive terminal:
Sometimes, non-interactive shells or certain environments (like cron jobs) might strip out escape sequences. If you're running the script in an interactive terminal (like a terminal emulator), try running the script there.

Updated Example:

Here’s how the .sh script should look:
```bash
#!/bin/bash

# Use \033 instead of \e for compatibility
printf "\033[35m=========================================================\n"
```
### Additional Debugging:

If it still doesn't work, try testing it directly in your terminal to see if it outputs colored text:
```bash
printf "\033[35mHello World in Magenta\033[0m\n"
```

This test will help verify if the terminal itself is supporting the escape codes.




