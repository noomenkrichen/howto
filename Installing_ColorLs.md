## ✅ Step-by-step Installation for Ubuntu:

### 1. **Install Ruby**

Colorls is a Ruby gem, so you need Ruby installed.

```bash
sudo apt update
sudo apt install ruby-full
```

Verify it:

```bash
ruby -v
```

---

### 2. **Install the required dependencies**

Colorls uses some native extensions and may require a few development tools:

```bash
sudo apt install build-essential
```

---

### 3. **Install the Colorls gem**

```bash
sudo gem install colorls
```

If you're getting permission errors, you can use:

```bash
sudo gem install colorls --no-document
```

---

### 4. **Install Nerd Fonts (for icons)**

Colorls uses fonts that include special glyphs.

You can install a Nerd Font (e.g., Hack Nerd Font):

```bash
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts
wget https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Hack.zip
unzip Hack.zip
fc-cache -fv
```

Then set your terminal font to "Hack Nerd Font" or similar in your terminal emulator's settings.

---

### 5. Enable and Test Colorls

Now run:

```bash
colorls
```

You can use flags like `-lA` for a better look:

```bash
colorls -lA
```

---

### 6. (Optional) Create alias for convenience

Add this to your `~/.bashrc` or `~/.zshrc`:

```bash
alias ls='colorls'
```

Then reload your shell:

```bash
source ~/.bashrc
```
