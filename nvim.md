# How to use Kickstart.nvim

## Basic utils | ripgrep | xclip
sudo apt install git make unzip gcc ripgrep xclip -y

## Installing Nerd Font - I do use this one :D
wget -P ~/.local/share/fonts https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/DepartureMono.zip \
&& cd ~/.local/share/fonts \
&& unzip DepartureMono.zip \
&& rm DepartureMono.zip \
&& fc-cache -fv

## Installing kickstart.nvim
git clone https://github.com/malcolnlmr/kickstart.nvim.git "${XDG_CONFIG_HOME:-$HOME/.config}"/nvim

Then let lazy.nvim do the rest for you
