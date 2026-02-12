# How to Delete a File or a Folder using Terminal Command ?
## Windows PowerShell
### Delete a File:
```sh
rm file_name.ext
```
or 
```sh
del file_name.ext
```
### Delete Empty Folder
```sh
rm folder_name
```
or
```sh
rmdir folder_name
```
Deletes only empty directories; fails immediately if contents exist (safer than rm).
### Delete Non-Empty Folder
This recursively deletes the folder and all contents (files, subfolders) by traversing the directory tree depth-first.
```sh
rm folder_name -r
```
Combines -r (recursive) with -f (force, no prompts) and -o (short for -Force in PowerShell alias).
```sh
rm folder_name -r _fo
```
### Comparison Table
| Command	| Handles Non-Empty?	| Prompts?	| PowerShell | Equivalent	Risk Level |
|---------|---------------------|-----------|------------|-----------------------|
| rm -r	| Yes (recursive)	| Yes (often)	| rm -Recurse	| High |
| rm -r -fo	| Yes (recursive)	| No	| rm -Recurse -Force	| Very High |
| rmdir	| No (empty only)	| No	| rmdir or rm (empty check)	| Low |

## Linux
### Delete a File or an Empty Folder:
```bash
rm file_name.ext
rm folder_name
```
### Delete Non-Empty Folder
```bash
rm -rf folder_name
```
