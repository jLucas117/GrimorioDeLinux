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
>control + alt + f2-f6(termianl) | f7(ambiente gráfico no meu caso i3wm)
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

>>2.3 - Formatar e montar as partições no sistema

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

>>2.3 Mirrors para o pacman

***