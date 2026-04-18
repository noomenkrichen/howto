# How to install ZSH terminal?
## exexcute the following command in your bash terminal:
```bash
sudo apt update
sudo apt upgrade
sudo apt install zsh
```
## after installation, you need to change your default terminal from BASH to ZSH using this command:
```bash
chsh -s $(which zsh)
```
## verify the changes using this command:
```bash
echo $SHELL
```
## to revert back to BASH at any time use the following command:
```bash
chsh -s $(which bash)
```
# How to install Oh my ZSH? this is optional
## to install Oh my ZSH, run the following command:
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
# How to install Power Level 10k themes?
## for manual installation without using the Oh my ZSH, run the following command to clone the reposotiry:
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
```
then run this command to add the source to your .zshrc file
```bash
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
## for automated installation with Oh my ZSH, run the following command to clone the reposotiry:
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```
then open your .zshrc file and add change the ZSH_THEME to "powerlevel10k/powerlevel10k"
```bash
vim ~/.zshrc
```
