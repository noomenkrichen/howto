# How to code in C++ in Ubuntu ?
build-essential is normally installed by default in Ubuntu

## Step 1: Install the C++ Compiler (g++) and tools 
Ubuntu does not come with a C++ compiler preinstalled. You can install the essential build tools, including the g++ compiler, using the terminal: 
### Open your terminal (press Ctrl + Alt + T).
### Update your package lists:
```bash
sudo apt update
```
### Install the build-essential package, which provides the g++ compiler and other required development libraries:
```bash
sudo apt install build-essential
```
### Verify the installation by checking the g++ version:
```bash
g++ --version
```
This should display the installed g++ version. 

## Step 2: Write your C++ program 
You can use a simple terminal-based editor like nano or a more advanced IDE/editor like Visual Studio Code, Code::Blocks, or Geany. 


Using the nano text editor (simple):
###Create and open a new file named hello.cpp using nano:
```bash
nano hello.cpp
```
### Type a simple "Hello, World!" program into the editor:
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
### Save the file by pressing Ctrl + O, then Enter, and exit the editor by pressing Ctrl + X. 

## Step 3: Compile your C++ program 
Use the g++ compiler in your terminal to compile the source file into an executable program: 
```bash
g++ hello.cpp -o hello
```
* g++: Invokes the GNU C++ compiler.
* hello.cpp: The name of your source code file.
* -o hello: Specifies the name for the output executable file. If omitted, the default executable name will be a.out. 

## Step 4: Run your C++ program 
Execute your compiled program from the terminal by typing the name of the executable, prefixed with ./: 
```bash
./hello
```
The output "Hello, World!" will be displayed in your terminal. 

## View Contents (Binary/Hex)
```bash
hexdump hello | head
```
Shows raw bytes. Or xxd hello on some systems.

## Disassemble (Assembly Code)
```bash
objdump -d hello
```
or
```bash
objdump -s hello
```
Displays machine instructions the CPU executes (not original C++ source).

## On Windows (if using MinGW/g++)
```text
dir hello.exe
hello.exe
objdump -d hello.exe
```
The executable is binary machine code—you can't recover exact source, but disassembly shows what it does.

## Using hexedit to edit executable files
Hexedit is a command-line hexadecimal editor for viewing and modifying binary files like your compiled hello executable.

### Install Hexedit
On Ubuntu/Debian:
```bash
sudo apt update && sudo apt install hexedit
```
### Basic Usage
```bash
hexedit hello
```
Opens your executable in hex/ASCII view (left: hex bytes, right: ASCII).

### Key Commands
* Navigation: Arrow keys, Page Up/Down, Home/End, < (file start), > (file end).
* Toggle view: Tab (hex ↔ ASCII).
* Edit: Type over hex digits or ASCII chars.
* Search: Ctrl+S (forward), Ctrl+R (backward).
* Undo: Backspace (last change), Ctrl+U (all changes).
* Save: F2 or Ctrl+X.
* Quit: Ctrl+C (no save), Ctrl+X (save & exit).
* Copy/paste: Ctrl+Space (set mark), Esc+W (copy), Ctrl+Y (paste).

Example: Edit your hello file—change bytes at offset 0x40119E (printf string) from 48 65 6C 6C 6F to 59 6F 21 to see "Yo!" instead. Save and run ./hello.

Caution: Editing executables can corrupt them—backup first (cp hello hello.bak).
