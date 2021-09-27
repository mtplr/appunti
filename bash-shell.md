Bash shell cookbook
=====================

Alcuni comandi utili in continuo aggiornamento che mi sono appuntato durante il mio apprendimento di linux, shell, 
WSL e cose varie.


Table of Contents
=================

   * [Importante (generale)](#importante-generale)
   * [Jolly](#jolly)
   * [Basic](#Basic)
   * [less](#less)
   * [Ranger](#ranger)
   * [Archivi](#archivi)
   * [nano](#nano)
      * [.nanorc](#nanorc)
   * [Ricerca file e manipolazione testo](#ricerca-file-e-manipolazione-testo)
      * [find](#find)
      * [grep](#grep)
      * [sed and rename](#sed-and-rename)
      * [altro](#altro)
   * [SSH](#ssh)
   * [rsync](#rsync)
   * [Miscellanea](#miscellanea)
   * [Python e Conda](#python)
   * [Bash](#bash)
      * [Alias, PATH](#alias-path)
      * [Scripting](#scripting)
      * [I/O Redirect](#io-redirect)
      * [Cicli for utili](#cicli-for-utili)
   * [WSL](#wsl)
   * [Risorse e link vari](#risorse-e-link-vari)
   

# Importante (generale)

* Ricorda che su **Windows** c'è `\r` (CRLF) e non `\n` (LF)!

* Sugli editor, se vuoi usare script o codice da leggersi su Linux, è necessario cambiare tutti i `\r`!

    * ...O usa `dos2unix` per convertire il file

* Se non ti fa copiare da shell  (ripo ranger o vim) **tieni premuto** <kbd>Shift</kbd> e seleziona col mouse!

* `Ctrl+C` annulla comando lanciato
* `Ctrl+Z` Getta comando lanciato in background (`fg` per rimetterlo in _foreground_)

* `G` in generale, vai in **FONDO** (nano Alt+/)
* `g` in generale, vai in **CIMA**

# Jolly

`?` = un char

`[0,1,2]` = un range

`*` = tutto

# Basic

`cd ..` vai in su

`ls` listi

`cdls() { cd "$@" && ls; }` funzione per fare cd+ls

`touch` crei un file e/o aggiorni timestamp

`rmdir` rimuove directory **vuota**

`rm` cancella un **file**

`rm -rf` rimuove ricorsivamente una cartella e i file all'interno in modo forzato (**attenzione!**)

`cp file1 file2` copia file

`cp -r` copia ricorsivamente una directory

`cp /home/asd/folder/{file{1,2},xyz,abc} /home/asd/dest` copia più files

`mv file1 file2` rename/move a file

`mkdir` crea cartella. Prende anche più argomenti e.g `mkdir dir1 dir2 dir3`

`pwd` will output the name of the present working directory

`ln -s source_file symbolic_link` creo un link simbolico (collegamento)

`type` restituisce alias o tipo di comando

```bash
$ type l
l is aliased to 'ls --color -lah --group-directories-first'

$ type cd
cd is a shell builtin
```

# less

Molto utile per leggere tutti i plain text.

`xzless` leggi testo compresso da `.xz`

`shift+g` vai in fondo

`q` quit

`?` ricerca **DAL BASSO (case sensitive!)**

`/` ricerca **DALL'ALTO**

`n` next

`N` prev

# Ranger

[Ranger Cheatsheet (by heroheman)](https://gist.github.com/heroheman/aba73e47443340c35526755ef79647eb)

`:` exec ranger command

`!` exec shell command

`r` open file with (puoi usarlo anche per operazioni tipo se hai uno script puoi fare r>script e lo apre con quello ecc)

`o` change sort order

`q` quit

`R` reload directory

`Backspace` mostra tutti i file nascosti con `.`

`spacebar` seleziona file (multicopia ecc)

`cw` o `F2` (meglio) rename current file

`:e nome.txt` crea un file e lo apre nell'editor di testo

`:chmod 775` cambia permessi a uno o più file selezionati

---

`shift+s` apre nuova shell **DENTRO** ranger (`exit` per uscire o `Ctrl+D`)

---

`Ctrl+n` new tab

`Ctrl+w` close tab

`tab` change tab

---

`yy` copy

`dd` cut

`pp` paste

---

`dD` delete

---

`/` or `?` search

`n` next

`N` prev

---

File association with Ranger: [How to change the default document viewer in ranger?](https://superuser.com/questions/1234893/how-to-change-the-default-document-viewer-in-ranger)

> For text files rifle (ranger's file opener) uses your default `$EDITOR`. You can set it to vim by adding the line export `EDITOR=vim` to your `~/.bashrc` or equivalent.
>
> For other file types you can configure rifle with the file `~/.config/ranger/rifle.conf`. First copy the defaults with `ranger --copy-config=rifle`. Then rearrange the programs (or add more) in the appropriate section so that your preferred application is at the top.
>
> In `~/.config/ranger/rifle.conf` i added `ext c|cpp|txt|md = vim "$@"` in the first line and `ext c|cpp|txt|md = atom "$@"` in the second line, to use atom as my alternative editor. So it worked, thanks! 

# Archivi

`xz`, comprime (solo file!) `.xz`

`unxz`, decomprime .xz

`p7zip` gestisce i `.7z` e anche gli `.zip`, basta metterlo come estensione, di default è `7z`

`7z a -r ../archivio *` crea `archivio.7z` della dir in cui sei, nella dir superiore

`7z e archivio.7z` estrae, uncompress

`tar -czvf archive.tar.gz /path/to/directory-or-file` to COMPRESS a gzip tar file (.tgz or .tar.gz)

`tar -xzvf file.tar.gz` to UNcompress a gzip tar file (.tgz or .tar.gz)

# nano

E' bello e user-friendly. [Qui il cheatsheet.](https://www.nano-editor.org/dist/latest/cheatsheet.html)

---

`Ctrl+S` Save current file

`Ctrl+X` Close

`Alt+\`	To TOP of buffer

`Alt+/`	To end of buffer

`Ctrl+Q` **back**ward search

`Alt+Q` Find next occurrence backward

`Ctrl+W` forward search

`Alt+W` Find next occurrence forward

`Alt+R` Start a replacing session

`Alt+U` undo

`Alt+R` redo

## .nanorc

Si può modificare `.nanorc` ([docu](https://www.nano-editor.org/dist/latest/nanorc.5.html)) aggiungendoci opzioni di configurazione interessanti, tipo:

```bash
set autoindent  # indent lines automatically
set mouse  # enable mouse support

# aggiungo supporto per la sintassi su Homebrew
include "/opt/homebrew/Cellar/nano/*/share/nano/*.nanorc"
```

# Ricerca file e manipolazione testo

## find

`find . -name "*.cpp"` will find all files ending in .cpp in the current
directory `.` and its subdirectories. To match the actual `.` or `*` symbols, you can escape them as `\.`
and `\*`. Case Sensitive! Per una parte sola usa `"OUT*"` tipo per `OUTPUT`

`find . -maxdepth 1 -type d` cerca tutte le sottocartelle (max 1 di profondità)

`find . -maxdepth 1 -type d \( ! -name . \) -exec bash -c "cd '{}' && pwd && script.sh" \;` cerca le cartelle ed eseguisci uno script dentro

`find "$(pwd)" -type f` printa tutti i percorsi assoluti dei file in una cartella (+ `> file.txt`) 

## grep

`grep -rni --include="*.txt" * -e 'ciao'` cerca ricorsivamente in QUELLA directory TUTTI i file txt la parola "ciao" case insensitive. Se vuoi la parola intera metti -w.

`grep -3 searchtext filename` will additionally output 3 lines before and
after any lines containing searchtext. Any number of lines can be used
here

`grep -B 3 -A 2 foo README.txt` will print all lines containing "foo" + 3 prima e 2 dopo (case INSENSITIVE)

`grep -v searchtext filename` will output all lines EXCEPT those
containing searchtext

`grep -r searchtext` will recursively search all files and folders starting
from the current directory

`grep -r --exclude='*.sql' searchtex dir/` will recursively search but EXCLUDE THOSE FILES es:
`grep -r --exclude='blacomcalc-output' BLA >> all_BLA`

`tac stdout | grep E0 -m 1` grep dal basso e si ferma alla prima occorrenza

`history | grep comando` trova tutti gli ultimi comandi relativi a quello

## sed and rename

**NOTA IMPORTANTE SU SED.**

[Non funziona ovunque allo stesso modo!](https://stackoverflow.com/questions/4247068/sed-command-with-i-option-failing-on-mac-but-works-on-linux) 

Per farlo andare allo stesso modo sia su Linux che su Mac (OS 10.9+) devi togliere lo spazio `sed -i'' -e ...`

`sed -i '' -e 's/T/F/g' FILE` sostituisci T con F linea per linea

`sed -i '' -e '142,145s/T/F/g' FILE` sostituisci T con F linea per linea dalla linea 142 alla 145 comprese

`sed -i 's/\r$//' filename` se hai un file fatto in windows serve per togliere l'a capo che ti può dare errore da 
linux, ATTENZIONE all'-i che vuol dire "in line mod"

`tac file.txt | sed '1,50s/T/F/g' | tac` replace every T with F in the last 50 lines of `file.txt`

`sed '/PATTERN/q' FILE`, stampa tutto il file finché non trova `PATTERN`.

> for each line, we look if it matches `/PATTERN`:
> * If yes, we print it and quit 
> * Otherwise, we print it \
> This is the most efficient solution, because as soon as it sees PATTERN, it quits. Without q, sed would continue to 
> read the rest of the file, and do nothing with it. For big files it can make a difference.

`rename 's/chcl3/DMSO/' *` rinomina tutti i file della cartella contenenti `chcl3` con `DMSO`. Per printare solo il risultato basta aggiungere `-n`.

## altro

`diff file1 file2` will output the lines which differ between the two files.
The lines from `file1` will be prepended with `<`  and the lines from
`file2` with `>`.

`split -dl 47 crest_conformers.xyz conformer --additional-suffix=.xyz` splitta un file di testo prendendo le prime 47 righe incluse, usi come suffisso `conformer` e ci appende un numero (`-d`). L'output sarà: `conformer00.xyz conformer01.xyz conformer02.xyz ...`. Se vuoi splittare DALLA 47 inclusa, metti `dl -46`.

`tac` è `cat` al contrario (stampa dal fondo)

# SSH

`scp` copia sicura SSH (`-r` per le cartelle) e.g. `scp file matteo@server:~/file.txt ~/Desktop`

`kill -9 $(pgrep bash)` per killare tutte le active windows se hai il problema di avere più `pts` aperti e non sai come chiuderli.

```bash
# Per salvare un server SSH con X11 e mettere un alias
# nano ./.ssh/config

Host nomealias
        HostName 000.000.000.000
        User user
        ForwardX11Trusted yes
        ForwardX11 yes
        Port 22
        
# poi lanciarlo con:
# ssh user@nomealias
```

# rsync

**IMPORTANTE**

* `~/Backup/` copia *il contenuto* della cartella
* `~/Backup` copia *la cartella* stessa

[Come usare rsync, esempi pratici](https://devdev.it/comando-rsync-esempi-pratici-144/), Luca Murante:

`rsync -zvrah ~/Desktop/devdev /Volumes/DiscoEsterno/Backup`

    -v significa verbose, avremo più informazioni a schermo
    
    -r significa recursive, con questa opzione rsync leggerà nelle cartelle
    
    -h trasforma in byte in un formato più leggibile (es. Mb, Gb)
    
    -a significa archive. Questa modalità corrisponde a scrivere -rlptgoD e riporta tutte le condizioni originali del file dalla destinazione alla sorgente come timestamp, link simbolici, permessi, proprietario e gruppo
    
    -z dice a rsync di comprimere i dati trasmessi
    
    --delete dice a rsync di cancellare sulla destinazione i file non presenti nella sorgente
    
    --exclude dice a rsync quali file o directory ignorare
    
    --progress mostra a schermo il trasferimento dei file
    
    -u imposta che i file presenti in destinazione più nuovi di quelli contenuti in sorgente vengano ignorati

`--delete` per eliminare i file nella destinazione se non ci son più (copia 1:1)

`--dry-run` per **testare** il funzionamento e fare una simulazione

`--max-size=100k` per backuppare tutto sotto i 100 KiB

```bash
# rsync personalizzato per server SSH
rsync -zvrah --delete --progress user@server:/home "/mnt/c/Backup"
```

Per syncare [più sources insieme](https://unix.stackexchange.com/questions/368210/how-to-rsync-multiple-source-folders):

```bash
rsync -vap --progress --stats root@server:{/etc,/root/backups,/home/ultralazer} /mnt/bigdrive
```

# PDF

`gv` per aprire `.ps` e `.pdf` files via X11 in modo leggero e _veloce_

`convert -density 200x200 -quality 60 -compress jpeg file1.pdf file2-compressed.pdf` per comprimere un'immagine PDF in jpg e quindi in PDF

`gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dDownsampleColorImages=true -dColorImageResolution=150 -dNOPAUSE  -dBATCH -sOutputFile=output.pdf INPUT.pdf` per comprimere un PDF


# Miscellanea

`yes | comando` dice sì a tutto

`yes n | comando` dice no a tutto

`neofetch` info belle di sistema con il logo in ASCII (funziona anche su MacOS)

`htop` task manager

`w` chi è connesso e che fa

`du -hc . | sort -rh | head -20` spazio, peso occupato dalla roba qua (.)

`chmod +x (775)` rendi il file eseguibile (e tutto il resto) da: me, group, not all (7= rwx, 5=rw)

`history` storico dei comandi shell

`!n` ripete l'n-esimo comando

`curl ipinfo.io` vede le info sul tuo IP

`printf '%s\n' "${PWD#*/}"` printa il nome della cartella in cui sei (basename)

`cat /proc/cpuinfo | head -18` print CPU info (of the first one)

`free -h` print RAM

`df -h` print disk space

`brew bundle dump --force --file=~/Folder/Brewfile` create a `Brewfile` with all updated Homebrew packages there

`sudo scutil --set HostName 'yourHostName'` per cambiare `nome@host` nel prompt

calcolatrice (`bc`)

```bash
$ bc <<<"236-192"
44
``` 

"Finder is using it; can't eject" problem

```
[matteo@aria ~]$ lsof /Volumes/
.timemachine/        Macintosh HD/        Disk/
[matteo@aria ~]$ lsof /Volumes/WindscribeInstaller/
COMMAND  PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
lsd      624 matteo    4r   DIR   1,22      102   20 /Volumes/Disk/Disk.app
lsd      624 matteo    5r   DIR   1,22      102   20 /Volumes/Disk/Disk.app
Finder  3370 matteo   16r   DIR   1,22      102   20 /Volumes/Disk/Disk.app
Finder  3370 matteo   18r   DIR   1,22      102   20 /Volumes/Disk/Disk.app
[matteo@aria ~]$ kill 624 3370
```

# Python

`python -c "import sys; print(sys.path)"` per vedere dov'è il path python giusto

`#!/usr/bin/env python3` da mettere come shebang all'inizio dello script python per farlo interpretare corretamente 
a linux indipendentemente dalla distribuzione

## argv

`sys.argv[1]` Per passare il **primo comando** da shell in modo *quick-and-dirty* allo script python. Il primo argument (`[0]`) è sempre il nome dello script, dal secondo (`[1]`) ci sono le vars passate da shell.

```python
#!/usr/bin/python

import sys

print 'Number of arguments:', len(sys.argv), 'arguments.'
print 'Argument List:', str(sys.argv)
```

Output:

```bash
$ python test.py arg1 arg2 arg3

Number of arguments: 4 arguments.
Argument List: ['test.py', 'arg1', 'arg2', 'arg3']
```

## Conda
Comandi essenziali **Conda** (miniforge):
```bash
conda create --name myenv 

# with a specific version 
conda create --name myenv

# to activate
conda activate myenv

# to install packages
conda install -n myenv scipy=0.15.0 

#(or without version)
conda install -n myenv scipy=0.15.0 

conda info # displays info
```

# Bash 

## Alias, PATH

[Vedi `.bashrc` e `.alias` nei dotfiles per comandi personalizzati]

`alias alias_name="command_to_run"` crea un alias (attenzione agli **spazi**!)

`export PATH=$PATH:percorso` aggiungi la cartella al `$PATH` linux (da aggiungersi alla fine di `.bashrc`)

`echo $PATH` per vedere i percorsi del `$PATH`

## Scripting

`#!/bin/bash` shebang da mettere sempre

`$1` usalo come primo argomento variabile nel `.sh` (sarà il primo comando passato da terminale e così via, `$1, $2, $3...`)

`$*` se vuoi passare più argomenti (tipo `arg1 arg2 arg3`)

`$_` è il parametro più recente

`nano script.sh && chmod +x $_` crea un nuovo script e lo rende eseguibile

`VAR=$(comando)` metti il comando dentro questa variabile

Per splittare un comando singolo in più linee (attenzione al LF! attenzione ci deve essere uno spazio prima ma NON dopo!)
```bash
# dopo il \ non ci sono spazi, prima sì!

./pyMergeColumns.py --symbol " " "monomer_vs_solvents.txt"  \
"../gas/IR-norm.dat" \
"../acetone/IR-norm.dat" \
"../DMSO/IR-norm.dat" \
"../chcl3/IR-norm.dat"
```

Se vuoi creare un file .txt con testo da bash:

```bash
cat > file.txt <<EOF
asd
asdas; dasd
Prova Foo
Bar>!
EOF
```

## I/O Redirect

`>` redirect to (**no errors**)

`&>` redirect to (**with** output errors)

`>>` append to (if already exist)

`&&` "lets you do something based on whether the previous command completed successfully - that's why you tend to see it chained as `do_something && do_something_else_that_depended_on_something`." [source e operatori](https://stackoverflow.com/questions/4510640/what-is-the-purpose-of-in-a-shell-command)

## Cicli for utili

`for d in */; do cp txt.txt "$d"; done` copia un file dentro ogni cartella

```bash
# Per le directories:

for d in */; do		
    echo "$d"
done
```

```bash
# To launch a command in all subdirectory (depth = 2)
# () opens a sub-shell !!!

for dir1 in *; do
	for dir2 in $dir1/*; do
		if [[ -d "$dir2" ]]; then
		    (cd "$dir2" && COMANDO && pwd);
		fi;
	done
done
```

# WSL

[Windows Subsystem Linux](https://docs.microsoft.com/it-it/windows/wsl/install-win10) (WSL) è bellissimo ed è integrato 
perfettamente in Windows 10, anche se fa storcere il naso a 
molti _puristi_.

`wsl.exe --shutdown` (da **PowerShell!**) per fare il reboot di linux (WSL) su Windows senza fare il reboot di Windows

`wslpath -a` restituisce il path utilizzabile da WSL (linux):

```bash
$ wslpath -a 'C:/Robe'
$ 'mnt/c/Robe'
```

Di default non funziona l'assegnazione dei permessi con `chmod`. Per farlo è necessario abilitarlo da shell: 
[Unable to change file permissions on Ubuntu Bash for Windows 10](https://superuser.com/questions/1323645/unable-to-change-file-permissions-on-ubuntu-bash-for-windows-10)

```bash
$ sudo nano /etc/wsl.conf

# aggiungi questo
[automount]
options = "metadata"

# reboot WSL 
```

OT: (comunque a Marzo 2021 sono passato a Mac perché non ce la facevo più col crapware. WSL resta comunque l'unica cosa bella di microsoft insieme a VSCode, asd.) 

# Risorse e link vari

* [Introduction to Scientific Computing](https://gitlab.com/eamonnmurray/IntroToScientificComputing/-/blob/master/README.md). Eccellente risorsa schematica per coloro che si affacciano a linux, allo scientific computing e alla chimica computazionale.
* [Tutorial interessante sulle regex (Youtube)](https://www.youtube.com/watch?v=sa-TUpSx1JA)
