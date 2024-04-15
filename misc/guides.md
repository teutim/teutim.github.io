# Guides for myself

## Rust

### Show assembly output

#### Specific function

For printing assembly output,\
where FUNCTION‌ is a function in crate CRATE:

1. Run ```cargo install cargo-show-asm``` once,
2. run ```cargo asm <CRATE>::<FUNCTION>```.

Source: <a href="https://stackoverflow.com/a/54287770" target="_blank">https://stackoverflow.com/a/54287770</a> \
Accessed: 2024-04-15

#### Whole crate

For printing assembly of a crate,\
where MODE is element of {```release```, ```debug```},\
where LMODE is element of {```--<MODE>```, ``` ```}:

1. Run ```cargo rustc <LMODE> -- emit asm```,
2. run ```ls target/<MODE>/deps/<CRATE>-<HASH>.s```.

Source: <a href="https://stackoverflow.com/a/39220789" target="_blank">https://stackoverflow.com/a/39220789</a> \
Accessed: 2024-04-15

## Building Ogre

### Erstellen des Ogre-SDKs
1. Runterladen des letzten Ogre-Releases.
2. Entpacken der Zip-Datei.
3. Im ogre-Ordner befindet sich eine CMakeLists.txt
   Dort muss der CMAKE_BUILD_TYPE auf "Release" gesetzt werden.
   Manuell können weitere Einstellungen vorgenommen werden.
4. Im ogre-Ordner muss ein out-Ordner erstellt werden und in diesem ein build-Ordner.
5. Im build-Ordner startet man die PowerShell und nutzt den Befehl

```cmake ../.. -G "Visual Studio 17 2022" -DCMAKE_BUILD_TYPE=Release```

Danach den folgenden Befehl:
```cmake --build . --config Release```

Danach den folgenden Befehl:
```cmake --build . --config Release --target install```

### Erstellen eines auf dem SDK basierenden Projekts
1. Anlegen eines neuen CMake-Projekts mit Visual Studio
2. Kopieren von

```set(OGRE_DIR <SDK-Pfad>)```

```find_package(OGRE REQUIRED COMPONENTS Bites CONFIG)```

```target_link_libraries(<Projektname> OgreBites)```

in den PROJEKTORDNER, der die Quelldateien enthält.
Projektname ist der Name des Targets, das zur späteren Exe wird.
Der Einfachheit halber der Name des PROJEKTORDNERS.
SDK-Pfad ist der Pfad des `sdk/CMAKE/-Unterordners` unter dem build-Ordner. (Siehe oben)

3. Kopieren des Inhalts der Datei, die unter `ogre/Tutorials/Bootstrap.cpp`
zu finden ist in die `<Projektname>.cpp`-Datei im CMAKE-Projektordner.



<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://teutim.github.io/misc/guides.html">Guides for myself</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/teutim">teutim</a> is licensed under <a href="https://creativecommons.org/licenses/by-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-SA 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1" alt=""></a></p>
