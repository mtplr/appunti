# Linux shell cookbook 

Alcuni comandi utili in continuo aggiornamento che mi sono appuntato durante il mio apprendimento di linux, shell, 
WSL e cose varie.

## Importante (generale)

* Ricorda che su **Windows** c'è `\r` e non `\n`!

* Sugli editor, se vuoi usare script o codice da leggersi su Linux, è necessario cambiare tutti i `\r`!

    * ...O usa `dos2unix` per convertire il file

* Se non ti fa copiare da shell  (ripo ranger o vim) **tieni premuto** <kbd>Shift</kbd> e seleziona col mouse!

* `Ctrl+C` annulla comando lanciato
* `Ctrl+Z` Getta comando lanciato in background (`fg` per rimetterlo in _foreground_)

* `G` in generale, vai in FONDO (nano Alt+/)
* `g` in generale, vai in CIMA

## Alias e comandi personalizzati

[Vedi `.bashrc`]

---

`l` listo tutto in ordine di cartelle (`alias l="ls --color -lah --group-directories-first"`)

`r` apro ranger (`alias r="ranger"`)

## Navigazione

`cd ..` vai in su

`ls` listi

`ll` listi tutto dettagliato (`-h` = human readable, vedi i GB etc)

`touch` crei un file e/o aggiorni timestamp

`rmdir` rimuove directory **vuota**

`rm` cancella un **file**

`rm -rf` rimuove ricorsivamente una cartella e i file all'interno in modo forzato (**attenzione!**)

`cp file1 file2` copia file

`cp -r` copia ricorsivamente una directory

`cp /home/asd/folder/{file{1,2},xyz,abc} /home/asd/dest` copia più files

`scp` copia sicura ssh (`-r` per le cartelle) fallo da locale **VERSO** il server. Ormai quasi deprecato. Usare `rsync`.

`mv file1 file2` rename/move a file

`mkdir` crea cartella

`pwd` will output the name of the present working directory

## less

Molto utile per leggere tutti i plain text.

`xzless` leggi testo compresso da `.xz`

`q` quit

`?` ricerca **DAL BASSO (case sensitive!)**

`/` ricerca **DALL'ALTO**

`n` next

`N` prev

## Ranger

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

`S` apre nuova shell **DENTRO** ranger (`exit` per uscire o `Ctrl+D`)

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

File association with Ranger: [informazioni da Superuser.com](https://superuser.com/questions/1234893/how-to-change-the-default-document-viewer-in-ranger)

> For text files rifle (ranger's file opener) uses your default `$EDITOR`. You can set it to vim by adding the line export `EDITOR=vim` to your `~/.bashrc` or equivalent.
>
> For other file types you can configure rifle with the file `~/.config/ranger/rifle.conf`. First copy the defaults with `ranger --copy-config=rifle`. Then rearrange the programs (or add more) in the appropriate section so that your preferred application is at the top.
>
> In `~/.config/ranger/rifle.conf` i added `ext c|cpp|txt|md = vim "$@"` in the first line and `ext c|cpp|txt|md = atom "$@"` in the second line, to use atom as my alternative editor. So it worked, thanks! 

## Archivi

`xz`, comprime (solo file!) .xz

`unxz`, decomprime .xz


## Jolly

`?` = un char

`[0,1,2]` = un range

`*` = tutto


## nano

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

## Ricerca file e testo

### find

`find . -name "*.cpp"` will find all files ending in .cpp in the current
directory `.` and its subdirectories. To match the actual `.` or `*` symbols, you can escape them as `\.`
and `\*`.

`find . -maxdepth 1 -type d` cerca tutte le sottocartelle (max 1 di profondità)

`find . -type d -exec script.sh "{}" ";"` cerca le cartelle ed eseguisci uno script dentro

### grep

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

`history | grep comando` trova tutti gli ultimi comandi relativi a quello

## SSH

`xauth list` vedi il magic cookie porta (non so a cosa serva) e mettila aprendo XLaunch (quindi XMing)

`kill -9 $(pgrep bash)` per killare tutte le active windows se hai il problema di avere più `pts` aperti e non sai 
come chiuderli.

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

## rsync

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

```bash
# rsync personalizzato per server SSH
rsync -zvrah --delete --progress user@server:/home "/mnt/c/Backup"
```



## Miscellanea

`screenfetch` info belle di sistema con il logo in ASCII 

`htop` task manager

`w` chi è connesso e che fa

`du -hc . | sort -rh | head -20` spazio occupato dalla roba qua (.)

`ln -s source_file symbolic_link` creo un link simbolico (collegamento)

`export PATH=$PATH:percorso` aggiungi la cartella al `$PATH` linux (da aggiungersi alla fine di `.bashrc`)

`echo $PATH` per vedere i percorsi del `$PATH`

`chmod +x (775)` rendi il file eseguibile (e tutto il resto) da: me, group, not all (7= rwx, 5=rw)

`history` storico dei comandi shell

`!n` ripete l'n-esimo comando

`diff file1 file2` will output the lines which differ between the two files.
The lines from `file1` will be prepended with `<`  and the lines from
`file2` with `>`.

`sed -i 's/\r$//' filename` se hai un file fatto in windows serve per togliere l'a capo che ti può dare errore da 
linux, ATTENZIONE all'-i che vuol dire "in line mod"

`sed '/PATTERN/q' FILE`, stampa tutto il file finché non trova `PATTERN`.

> for each line, we look if it matches `/PATTERN`:
> * If yes, we print it and quit 
> * Otherwise, we print it \
> This is the most efficient solution, because as soon as it sees PATTERN, it quits. Without q, sed would continue to 
> read the rest of the file, and do nothing with it. For big files it can make a difference.

`rename 's/chcl3/DMSO/' *` rinomina tutti i file della cartella contenenti `chcl3` con `DMSO`. Per printare solo il risultato basta aggiungere `-n`. 

`tac` è `cat` al contrario (stampa dal fondo)

`gv` per aprire `.ps` e `.pdf` files via X11 in modo leggero e _veloce_

`tar -czvf archive.tar.gz /path/to/directory-or-file` to COMPRESS a gzip tar file (.tgz or .tar.gz)

`tar -xzvf file.tar.gz` to UNcompress a gzip tar file (.tgz or .tar.gz)

`alias alias_name="command_to_run"` crea un alias (attenzione agli **spazi**!)

`curl ipinfo.io` vede le info sul tuo IP

`>` redirect to 

`>>` append to

## Python path

`python -c "import sys; print(sys.path)"` per vedere dov'è il path python giusto

`#!/usr/bin/env python3` da mettere come shebang all'inizio dello script python per farlo interpretare corretamente 
a linux indipendentemente dalla distribuzione

## Bash scripting

`#!/bin/bash` shebang da mettere sempre

`$1` usalo come primo argomento variabile nel `.sh` (sarà il primo comando passato da terminale e così via, `$1, $2, $3...`)

`$(comando)` metti il comando dentro questa variabile 

## Cicli for utili

`for filename in */gas/BLA-1.dat; do ...`

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
:~$ wslpath -a 'C:/Robe'
:~$ 'mnt/c/Robe'
```

# Risorse e link vari

* Eccellente risorsa schematica per coloro che si affacciano a linux *e/o* alla chimica computazionale: [qui by Éamonn 
  Murray](https://gitlab.com/eamonnmurray/IntroToScientificComputing/-/blob/master/README.md).

* [Tutarial interessante sulle regex (Youtube)](https://www.youtube.com/watch?v=sa-TUpSx1JA)
* [Opzioni .nanorc](https://linuxhint.com/configure_nano_text_editor_nanorc/)
* [How to Run Graphical Linux Desktop Applications from Windows 10’s Bash Shell](https://www.howtogeek.com/261575/how-to-run-graphical-linux-desktop-applications-from-windows-10s-bash-shell/)
