# Grimório De Linux
>comandos que utilizo e howtos aleatorios

## Sumário
-[Comandos Úteis](#comandos-úteis)

-[Comandos I3WM](#comandos-i3wm)

-[Instalando Arch](#instalando-arch)

## Comandos Úteis

>**Desligar o sistema**
>```
>sudo shutdown now
>```

>**Mostrar diretório atual**
>```
>pwd
>``` 

>**Trocar de diretório**
>```
>cd <dir>
>``` 
>>'~/' diretório home

>**remover arquivo**
>```
>rm <arquivo>
>``` 
>>remover diretório
>>>```
>>>rmdir <diretório>
>>>```
>>remover diretório com arquivos dentro 
>>>```
>>>rm -r <diretório>
>>>``` 
>>>o -r significa recursivo

>**Copiar arquivo**
>```
>cp <arquivo> <destino>
>``` 
>>para pastas com arquivos dentro -r

>**Mover arquivo ou renomear**
>```
>mv <arquivo> <destino-final+arquivo>
>``` 

>**Hibernar**
>```
>systemctl suspend
>```

>**Mostrar arquivos com flag -a para incluir arquivos ocultos**
>>Que são arquivos com '.' no início do nome
>```
>ls -a
>``` 

>**Dar permissão de execução a um arquivo**
>```
>chmod +x <arquivo>
>``` 

>**Trocar para super usuário ou voltar ao usuário anterior**
>```
>su <username>
>``` 

>**Matar task no terminal**
>Ctr + alt + \ ou /

>**Colar texto no terminal**
>ctr + shift + v

***
## Comandos I3WM

>**Fullscreen**
>Mod + f

>**Abrir terminal**
>Mod + enter

>**Matar tela ativa**
>alt + shift + q

>**Trocar para terminal ou voltar para ambiente gráfico**
>>dessa forma é possível consertar alguns erros como alterar arquivos de config que deixam em loop de login e coisas assim.
>```
>control + alt + f2-f6(terminal) | f7(ambiente gráfico no meu caso i3wm)
>```

>**Executar o dmenu se tiver e for o atalho padrão**
>mod + d

***
## Instalando Arch

>Instalação foi feita no início de 2026 algumas coisas podem mudar caso não for atualizado.

>(Existem n modos de instalar esse é apenas o modo que eu fiz vendo alguns tutoriais no yt vou likar os que lembro ter usado)

>!Antes de instalar checar modo da BIOS e desabilitar secure boot

>1 - Setar o layout do do teclado
>'''
>localectl set-keymap br-abnt2
>''' 

>2 - Particionamento

>>2.1 - Checar partições reconhecidas do sistema

>>```
>>lsblk 
>>```

>>2.2 - Utilizar cfdisk para particionar

>>```
>>cfdisk <partition>
>>```
>>>Existem várias maneiras de particionar seu sistema, exemplo de uma:
>>>4GB-8GB swap partition
>>>(se o modo de boot for EFI o mais comum hoje em dia) 1GB  EFI partition
>>>main partition Linux filesystem (todo o resto da memória)

>>2.2 - Formatar e montar as partições no sistema

>>```
>>mkfs.fat -F32 /dev/nomedapartiçãoEFI
>>```
>>```
>>mkfs.ext4 /dev/nomepartiçãoMAIN
>>```
>>```
>>mount /dev/nomepartiçãoMAIN /mnt
>>```
>>```
>>mkdir /mnt/boot
>>```
>>```
>>mount /dev/nomepartiçãoEFI /mnt/boot
>>```
>>```
>>mkswap /dev/nomepartiçãoSWAP
>>```

>>Você então pode utilizar lsblk para checar se as partições foram devidamente montadas em seus diretórios.

>3 - Mirrors para o pacman

>>o arquivo de configurações do mirror está em /etc/pacman.d/mirrolist
>>(você pode abri-lo com -less <arquivo>)
>>então deve gerar o conteúdo deste arquivo com comando reflector
>>```
>>reflector --latest 5 --country Brazil --protocol http, https --sort rate --save /etc/pacman.d/mirrorlist
>>```
>>(você pode mostrar o conteúdo deste arquivo com cat <arquivo>)

>4 - Baixar arquivos iniciais

>>```
>>pacstrap -K /mnt base-linux linux-firmware networkmanager vim base-devel
>>```
>>(adicione intel-ucode se tiver intel)

>5 - Salvar particionamento na memória
>>```
>>genfstab -U /mnt >> /mnt/etc/fstab
>>```

>6 - Login no sistema

>>```
>>arch-chroot /mnt
>>```

>7 - Setar informação de localização

>>Você pode digitar o comando abaixo e pressionar tecla tab ao invés de executar para ver diferentes opções
>>```
>>ln-sf /usr/share/zoneinfo
>>```
>>no meu caso:
>>```
>>ln-sf /usr/share/zoneinfo/Brazil/West /etc/localtime
>>```
>>```
>>hwclock --systohc
>>```
>>Nas próximas etapas será utilizado o programa vim para editar arquivos de texto
>>Agora você precisa localizar e descomentar (tirar a cerquilha ou jogo da velha) de sua localização neste arquivo
>>```
>>vim /etc/locale.gen
>>```
>>*no meu caso os dois pt_BR
>>dai executar o comando
>>```
>>locale-gen
>>```
>>Então abra com o vin
>>```
>>vim /etc/locale.conf
>>```
>>Escreva LANG= pt_BR.UTF-8

>8 - Setar nome do sistema
>>Crie o arquivo abaixo e escreva qualquer coisa dentro do mesmo que será o nome do sistema
>>```
>>vim /etc/hostname
>>```

>9 - Habilitar network manager ao inciar o sistema
>>```
>>systemctl enable NetworkManager
>>```
>>(!É importante ter certeza que instalou o network manager principalmente se você utiliza thetering usb ou wifi!)

>10 - Colocar senha no super usuário (que será utilizado para executar comandos que exigem este nível de credencial)

>>```
>>passwd
>>```

>>(!toda vez que utilizar o comando sudo é este password que devera digitar!)

>>11 - Adicionar um usuário 

>>```
>>useradd -m -G wheel, users <nome do usuário>
>>```
>>```
>>passwd <nome do usuário>
>>```

>>12 - Adicionar para o grupo de usuários wheel a opção de executar comandos como administrador (sudo)

>>```
>>sudo visudo
>>```

>>Você deve descomentar (tirar a cerquilha) onde tem Uncomment to allow members of group wheel to execute commands

>13 - Instalar o BootLoader
>>```
>>pacman -S grub efibootmgr
>>```
>>```
>>grub-install --target=X86_64-efi --efi-directory= /boot --bootloader-id=GRUB
>>```
>>```
>>grub-mkconfig -o /boot/grub/grub.cfg
>>```
>>```
>>pacman -S git man-db man-pages reflector
>>```

>>Eu acho que para adicionar dual boot é necessário descomentar GrubPickableOsProber in /etc/default/grub

>>14 Sair do chroot e reboot no sistema

>>```
>>exit
>>```
>>```
>>unmount -R /mnt
>>```
>>```
>>reboot
>>```

>15 Instalar Drivers da nvidia caso seja sua placa de vídeo
>>```
>>sudo pacman -S linux-headers linux-lts-headers 
>>```
>>```
>>sudo pacman -S nvidia-utils nvidia-settings
>>```
>>```
>>sudo pacman -S nvidia-dkms
>>```
>>```
>>reboot
>>```

>>16 - Instalar i3wm um terminal e um greeter

>>```
>>sudo pacman -S i3
>>```
>>Vai aparecer um menu de seleção, selecione todos menos o segundo item
>>```
>>sudo pacman -S dmenu kitty
>>```
>>```
>>sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
>>```
>>Dai habilitar ele quando o sistema inicia
>>```
>>systemctl enable lightdm 
>>```

>>17 para deixar o terminal kitty transparente
>>```
>>sudo pacman -S picom
>>```
>>Criar um diretório e arquivo de configurações do picom
>>```
>>mkdir ~/.config/picom
>>```
>>```
>>vim  ~/.config/picom/picom.conf
>>```

>>adicione as seguintes linhas 
>>```
>>backend = "xrender";
>>opacity-rule = ["85:class_g" = "kitty"];
>>```

>>Então você precisara setar o picom na configuração do i3wm (arquivo no final)

>>18 - Notificações, papel de parede e tirar print 
>>```
>>sudo pacman -S dunst feh flameshot
>>```

>>Para configurar e iniciar as notificações papel de parede colar esses comandos em ~/.config/i3/config
>>```
>>sudo vim ~/.config/i3/config
>>```
>>```
>>#notifications
>>exec_always --no-startup-id dunst
>>
>>#wallpaper
>>exec --no-startup-id feh --bg-fill  ~/pictures/wallpaper/nanachi.jpg
>>
>>#transparency picom
>>exec --no-startup-id picom --config /home/lucne/.config/picom/picom.conf
>>
>>#print
>>bindsym Print exec "flameshot gui" 
>>
>>```

>>Além disso com o ~/.config/i3/config e ~/.config/i3status/config você pode configurar a aparência das janelas e barra

***