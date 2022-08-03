
# Erstellen des Ogre-SDKs
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

# Erstellen eines auf dem SDK basierenden Projekts
1. Anlegen eines neuen CMake-Projekts mit Visual Studio
2. Kopieren von

```set(OGRE_DIR <SDK-Pfad>)```

```find_package(OGRE REQUIRED COMPONENTS Bites CONFIG)```

```target_link_libraries(<Projektname> OgreBites)```

in den PROJEKTORDNER, der die Quelldateien enthält.
Projektname ist der Name des Targets, das zur späteren Exe wird.
Der Einfachheit halber der Name des PROJEKTORDNERS.
SDK-Pfad ist der Pfad des `sdk/CMAKE/-Unterordners` unter dem build-Ordner. (Sieh oben)

3. Kopieren des Inhalts der Datei, die unter `ogre/Tutorials/Bootstrap.cpp`
zu finden ist in die `<Projektname>.cpp`-Datei im CMAKE-Projektordner.
