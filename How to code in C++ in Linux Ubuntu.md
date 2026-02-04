# How to code in C++ in Ubuntu ?
build-essential is normally installed by default in Ubuntu

## Step 1: Install the C++ Compiler (g++) and tools 
Ubuntu does not come with a C++ compiler preinstalled. You can install the essential build tools, including the g++ compiler, using the terminal: 
Open your terminal (press Ctrl + Alt + T).
Update your package lists:
bash
sudo apt update
Install the build-essential package, which provides the g++ compiler and other required development libraries:
bash
sudo apt install build-essential
Verify the installation by checking the g++ version:
bash
g++ --version
This should display the installed g++ version. 

## Step 2: Write your C++ program 
You can use a simple terminal-based editor like nano or a more advanced IDE/editor like Visual Studio Code, Code::Blocks, or Geany. 
Using the nano text editor (simple):
Create and open a new file named hello.cpp using nano:
bash
nano hello.cpp
Type a simple "Hello, World!" program into the editor:
cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
Save the file by pressing Ctrl + O, then Enter, and exit the editor by pressing Ctrl + X. 

## Step 3: Compile your C++ program 
Use the g++ compiler in your terminal to compile the source file into an executable program: 
bash
g++ hello.cpp -o hello_program
g++: Invokes the GNU C++ compiler.
hello.cpp: The name of your source code file.
-o hello_program: Specifies the name for the output executable file. If omitted, the default executable name will be a.out. 

## Step 4: Run your C++ program 
Execute your compiled program from the terminal by typing the name of the executable, prefixed with ./: 
bash
./hello_program
The output "Hello, World!" will be displayed in your terminal. 
