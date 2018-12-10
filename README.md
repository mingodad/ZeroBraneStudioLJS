# ZeroBraneStudioLJS
Port of ZeroBraneStudio from lua to ljs

Here is the conversion so far of ZeroBraneStudio from Lua to LJS https://github.com/mingodad/ljs .

The process was (there is a ljs script to automate it check-diff.ljs):

- Download ZeroBraneStudio-1.8 from https://github.com/pkulchenko/ZeroBraneStudio/archive/1.80.tar.gz

- Extract it in two places, one for the original "ZeroBraneStudio-1.80" and the other to the conversion to LJS "ZeroBraneStudio-1.80-LJS"

- Run lua2ljs on all *.lua files in ""ZeroBraneStudio-1.80-LJS" and remove the lua files
```
for fn in *.lua; do lua2ljs $fn > ${fn/\.lua/\.ljs}; rm $fn; done 
```
- Edit manually references of ".lua" to ".ljs"

- Edit manually to remove duplicated declarations reported by running and errors reported by ljsc
```
ljsc -p -l -l fname_ljs > /dev/null 
```

- Crate an alias for all lualibs/lexers/*.ljs because the lexlpeg expect files ending in ".lua":
```
for fn in *.ljs; do ln -s $fn ${fn/\.ljs/\.lua}
```

- Edit the script zstudio.sh to use ljs and disable background to be able to see any error:
```
(cd "$DIR"; bin/linux/$ARCH/ljs src/main.ljs zbstudio -cwd "$CWD" "$@") #&
```

- Add the ljs binaries in bin, in my case "bin/linux/x64" ljsjit ljs-51 and alias one of then to "ljs"

- Run the script zstudio.sh to see it working

- Fix errors reported and run "t/test.sh" and try fix anything reported


