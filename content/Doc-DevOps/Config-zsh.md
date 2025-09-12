`sudo apt install zsh`
#### Instalar repositorios
En la raíz, instalar todos los repos a continuación.
➜  ~   
##### Instalar Oh My Zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
##### Instalar Powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
###### Instalar zsh-syntax-highlighting:
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
###### Instalar zsh-autosuggestions:
git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
###### Instalar zsh-completions:
git clone https://github.com/zsh-users/zsh-completions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
###### Una vez instalados, verifica que la estructura de los plugins esté en ~/.oh-my-zsh/custom/plugins/. Deberías ver tres carpetas:
*zsh-syntax-highlighting*
*zsh-autosuggestions*
*zsh-completions*
###### Configura tu archivo .zshrc: Abre el archivo .zshrc
`nano ~/.zshrc`
###### Encuentra la línea que dice ZSH_THEME="robbyrussell" y cámbiala por:
`ZSH_THEME="powerlevel10k/powerlevel10k"`
###### Los plugins se habilitan añadiendo sus nombres en el archivo de configuración ~/.zshrc
Busca la línea que dice plugins=() y modifica su contenido
`plugins=(git colored-man-pages docker docker-compose zsh-syntax-highlighting zsh-autosuggestions zsh-completions)`
###### Aplica los cambios: 
`source ~/.zshrc`
Seguir los pasos de configuración y aplicar cambios otra vez.
##### Configurar por defecto zsh
*Ir a la preferencia del terminal -> comando -> ejecutar un cmd personalizado en vez de mi intérprete -> zsh*
###### O este comando, para cambiar el shell predeterminado Zsh: 
`chsh -s $(which zsh)`
###### Cerrar sesión y verificar shell
`echo $SHELL`
`echo $0`
###### Cambiar en la terminal actual, hace cambios momentáneos hasta que cierres la terminal.
`exec zsh`
# Opcional

##### Descarga fuentes
Descarga las fuentes recomendadas (como "Meslo Nerd Font"): Visita https://github.com/romkatv/powerlevel10k#fonts para descargar las fuentes. Luego instala la fuente que prefieras.
###### Configura tu terminal para usar la fuente instalada. Si usas GNOME Terminal, por ejemplo:
Abre las preferencias del terminal.
En la sección de "Texto", activa la opción "Usar una fuente personalizada" y selecciona la fuente que descargaste (por ejemplo, "Meslo Nerd Font").
##### Ejecuta config Powerlevel10k
`p10k configure`

