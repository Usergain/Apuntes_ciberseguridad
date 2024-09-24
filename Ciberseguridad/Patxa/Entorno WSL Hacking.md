Luego, los archivos de configuración de zshrc puedes modificarlos o coger dotfiles de mi repo de bspwm o algun otro que te guste. Y la config de p10k.zsh puedes generarla sola con `p10k configure` o copiando algun dotfile
Si tienes dudas me dices

`sudo apt-get install zsh lsd batcat`
`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.powerlevel10k` `echo 'source ~/.powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc`
`sudo git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /root/.powerlevel10k`

`sudo apt-get install -y zsh-syntax-highlighting zsh-autosuggestions` `sudo mkdir /usr/share/zsh-sudo` `cd /usr/share/zsh-sudo` `sudo wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh`

