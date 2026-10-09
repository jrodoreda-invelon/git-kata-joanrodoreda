# Bitácora de Aprendizaje - Git Kata

## Misión 0 - El campo de entrenamiento
- **Comandos ejecutados:** 
  `git clone ...`, `git add menu.md`, `git commit -m "..."`
- **Salida real obtenida:**
  *I did it on the previous session and didn't save it*
- **Respuestas a las preguntas:**
  - ¿Qué diferencia hay entre add y commit? 
    - git add = Prepara los cambios (los lleva al staging area). 
    - git commit = Sella y guarda esos cambios preparados en la historia de Git para siempre con un mensaje descriptivo.
  - ¿Y entre commit y push?
    - 'commit' guarda los cambios en tu PC, y 'push' los sube a GitHub.
  - ¿Qué es el “staging area” y por qué existe un paso intermedio?
    - Es una "zona de espera" (lo que preparas con git add). 
      Existe para que puedas elegir exactamente qué archivos quieres guardar en el próximo commit, 
      evitando empaquetar archivos a medias, pruebas o errores sin querer.
    
    - El comando git commit no elige ni filtra archivos; simplemente coge todo lo que ya esté en el staging area en ese momento y lo empaqueta de golpe. 
      Por lo tanto, el verdadero responsable de decidir qué entra en el commit (y qué se queda fuera) es el comando git add.

## Misión 1 — Ramas y commits sucios (a propósito)
- **Comando para hacer un desastre y luego limpiar:**
  `git switch -c feature/1-primeros-platos`
- **Salida real obtenida:**
  *Cambiado a nueva rama 'feature/1-primeros-platos'*

- **Hacemos 4 cambios diferentes y un commt por cambio respectivamente:**
  1	Añade - Ensalada bajo Primero	wip
  2	Añade - Gazpacho bajo Primero	fix
  3	Corrige una falta que hayas puesto	asdf
  4	Añade - Crema de calabaza bajo Primero	otra vez

- git add menu.md

- **Salida real obtenida:**
- 1. 
  *git commit -m "wip"*
[feature/1-primeros-platos fff3d2c] wip
 2 files changed, 1 insertion(+), 1 deletion(-)
 create mode 100644 BITACORA.md
- 2.
  *git commit -m "fix"*
[feature/1-primeros-platos 3872702] fix
 1 file changed, 1 insertion(+)
- 4.
  *git commit -m "otra vez"* 
## He tenido el error que he puesto puntos en cada plato para el paso de 'asdf' pero no he echo git add ni commit, por eso aparecen mas cambios
[feature/1-primeros-platos d09fd3d] otra vez
 1 file changed, 6 insertions(+), 5 deletions(-)

## Hago ahora el asdf pero quito los puntos que he puesto antes
- 3. 
  *git commit -m "asdf"*
[feature/1-primeros-platos 9513bd5] asdf
 1 file changed, 5 insertions(+), 5 deletions(-)

- git log --oneline
9513bd5 (HEAD -> feature/1-primeros-platos) asdf
d09fd3d otra vez
3872702 fix
59cb197 wip
c267fc5 vuelta a inicio
fff3d2c wip
00366e4 (origin/main, origin/HEAD, main) Add base menu
dbc4172 Initial commit

-  **Respuestas a las preguntas:**
  - ¿Qué le dice ese historial a alguien que revise tu PR?
    Pues le da la visión de que el codigo esta desordenado y poco bien estructurado 
    para entender bien los cambios que se hacen.
    Le hará tener que revisar el codigo para ver lo que se ha hecho porque los commits 
    no explican los cambios realizados.

  - git switch -c ≟ git checkout -b. ¿Son lo mismo? ¿Cuál es más moderno?
    Ambos hacen lo mismo, crean una nueva rama y te cambian a ella.
    `git switch -c` es mas moderno.
    Se creó `switch -c` porque checkout estaba demasiado sobrecargado y servía 
    para más de una cosa:
    cambiar de rama, descartar cambios en un archivo, viajar en el historial a un commit antiguo, etc.

## Misión 2 — amend: arreglar el último commit

- **Comandos ejecutados:** 
echo "- Crema de calabaza (vegana)" >> menu.md   # ajústalo a tu fichero
git add menu.md
git commit --amend -m "Add pumpkin cream to starters"
git log --oneline
- **Salida real obtenida:**
[feature/1-primeros-platos 000e696] Add pumpkin cream to starters
 Date: Fri Oct 9 10:10:05 2026 +0200
 1 file changed, 6 insertions(+), 5 deletions(-)

000e696 (HEAD -> feature/1-primeros-platos) Add pumpkin cream to starters
d09fd3d otra vez
3872702 fix
59cb197 wip
c267fc5 vuelta a inicio
fff3d2c wip
00366e4 (origin/main, origin/HEAD, main) Add base menu
dbc4172 Initial commit

- ¿Apareció un commit nuevo o cambió el que ya había?
    Pues se ve que no, se ha sobreescrito el commit "asdf" por "Add pumpkin cream to starters"
- Compara el hash del último commit antes y después del amend. ¿Es el mismo?
    Exactamente el mismo, solo cambia la descripción.
- ⚠️ Pregunta clave: ¿por qué amend es peligroso si YA habías hecho push?
    
Cuando modificas un commit con --amend, Git no edita el commit original, sino que lo destruye y 
crea uno totalmente nuevo con un identificador (hash) distinto.
Si ese commit original ya estaba subido a GitHub (porque hiciste push):
- El servidor remoto y tus compañeros tendrán una versión de la historia (con el hash antiguo).
- Tu ordenador tendrá una versión diferente (con el hash nuevo).
- Si intentas subir ese cambio, Git te dará un error. Si lo fuerzas (con --force), romperás 
  la línea de tiempo del resto del equipo, generando conflictos masivos la próxima vez que 
  intenten hacer pull o subir su propio código.

# Regla de oro de Git: Puedes hacer todo el amend, squash o rebase que quieras con los commits que 
# están solo en tu ordenador, pero nunca reescribas la historia de commits que ya han sido compartidos (pusheados) al remoto.

## Misión 3 — squash: la que no hiciste en CMS Jr.

 `git rebase -i main`
[HEAD desacoplado 7081f8d] Add starters section to the daily menu
 Date: Fri Oct 9 09:52:29 2026 +0200
 3 files changed, 126 insertions(+), 1 deletion(-)
 create mode 100644 BITACORA.md
Rebase aplicado satisfactoriamente y actualizado refs/heads/feature/1-primeros-platos.
# Lo que pasa es que se abre el editor con:
pick 1a2b3c4 wip
pick 2b3c4d5 fix
pick 3c4d5e6 asdf
pick 4d5e6f7 Add pumpkin cream to starters
# Y entonces tenemos que cambiar pick por s o squash:
pick   1a2b3c4 wip
squash 2b3c4d5 fix
squash 3c4d5e6 asdf
squash 4d5e6f7 Add pumpkin cream to starters
# Una vez guardas(^O) y sales(^X) se abre otro editor donde borramos todo y escribimos:
Add starters section to the daily menu

Lo que ha pasado no es que hayas "perdido" o eliminado los cambios de tu código, 
sino que Git ha fusionado (aplastado o squashed) esos 4 commits sucios que tenías 
(wip, fix, asdf...) en un único commit nuevo y totalmente limpio (7081f8d).

- Pega el git log --oneline de antes y de después.

`git log --oneline` -- ANTES --
000e696 (HEAD -> feature/1-primeros-platos) Add pumpkin cream to starters
d09fd3d otra vez
3872702 fix
59cb197 wip
c267fc5 vuelta a inicio
fff3d2c wip
00366e4 (origin/main, origin/HEAD, main) Add base menu
dbc4172 Initial commit

`git log --oneline` -- DESPUÉS --
00366e4 (origin/main, origin/HEAD, main) Add base menu
dbc4172 Initial commit

- ¿Cuántos commits quedan? ¿Se perdió algún cambio del fichero?
Quedan los commits hechos antes del último push, ahora tenemos un único commit 
para los commits de después haciendolo todo mejor estructurado. 
Los ficheros siguen idénticos, sin cambios.

- ¿Por qué un revisor prefiere 1 commit claro a 4 commits “wip”?
Pues evidentemente porque una mala explicación no ayuda en nada, el que tenga 
que revisar el código no tendrá información de los nuevos cambios y por lo 
tanto el efuerzo tendrá que ser mayor.

## Misión 4 — PR y merge
1. Ve a GitHub → Compare & pull request. 
2. Base main ← Compare feature/1-primeros-platos. 
3. Título claro + descripción: qué hace y cómo probarlo. 
4. Mergea el PR desde la web.

Hice primero un push para guardar en el Github los cambios hechos hasta el momento, "Antes de Mision 4".
Luego se me ha haceptado el PR y se ha creado el merge.
Entonces he hecho el cambio de rama a main, he hecho el pull y toda la rama main se ha actualizado.
git switch main
git pull
git log --oneline --graph --all

Y aqui podemos observar el arbol de git:
*   8422ee0 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
|\  
| * 0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
| * 7081f8d Add starters section to the daily menu
|/  
* 00366e4 Add base menu
* dbc4172 Initial commit

- ¿Aparece un “commit de merge”? ¿Qué es y por qué existe?
Sí, es un commit especial automático que se crea al fusionar dos ramas. A diferencia de un commit normal que 
tiene un solo ancestro, un commit de merge tiene dos padres: el último estado que tenía main y el último estado 
de tu rama feature/1-primeros-platos. En tu grafo se ve perfectamente representado por el nudo |\ que une ambas líneas.

Por qué existe: Sirve para preservar la historia real del proyecto. Documenta que ese código se desarrolló de 
forma paralela en una rama aislada y deja constancia del momento exacto en el que esos cambios fueron aprobados 
(vía Pull Request) e integrados en la línea principal de producción.


## Misión 5 — fetch vs pull 

# Paso 1 — toca main desde la web de GitHub.
He editado el README.md desde la misma web de github.
Ahora el remoto va por delante de el local.

# Paso 2 — en tu terminal, uno a uno, apuntando qué ves:

`git log --oneline main -1            # (A) tu main local`
8422ee0 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos

`git log --oneline origin/main -1     # (B) lo que tú CREES que hay en el remoto`
8422ee0 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
EXACTAMENT EL MATEIX, llavors s'equivoca perquè hi ha canvis al README.md. El nostre local mira la memòria que té de 
l'últim cop que s'ha connectat amb el Github (Internet).

`git fetch origin`
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Desempaquetando objetos: 100% (3/3), 1.04 KiB | 1.04 MiB/s, listo.
Desde https://github.com/jrodoreda-invelon/git-kata-joanrodoreda
   8422ee0..d28d6ad  main       -> origin/main

'(Fetch) La llamada al servidor: Al ejecutar git fetch origin, tu ordenador se conectó a GitHub, vio que había 
un commit nuevo (d28d6ad), descargó esos datos y actualizó únicamente su caché local (origin/main).'

`git log --oneline main -1            # (C) ¿cambió tu main?`
8422ee0 (HEAD -> main) Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos

`git log --oneline origin/main -1     # (D) ¿cambió origin/main?`
d28d6ad (origin/main, origin/HEAD) Update README with Mission 5 details

`git status                           # (E) ¿qué te dice ahora?`
En la rama main
Tu rama está detrás de 'origin/main' por 1 commit, y puede ser avanzada rápido.
  (usa "git pull" para actualizar tu rama local)

Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
        modificados:     BITACORA.md

sin cambios agregados al commit (usa "git add" y/o "git commit -a")

`git pull`
Actualizando 8422ee0..d28d6ad
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)

`git log --oneline main -1            # (F) ¿y ahora?`
d28d6ad (HEAD -> main, origin/main, origin/HEAD) Update README with Mission 5 details

- **Respuestas a las preguntas:**
    1. Después del fetch, ¿cambió tu main? ¿Cambió origin/main?
  Després del fetch, el teu main local no va canviar. L'origin/main sí que va canviar
  (es va actualitzar al nou commit de GitHub).
  
    2. ¿Qué es exactamente origin/main? ¿Está en tu disco o en GitHub?
  L'origin/main és una branca de seguiment remot que actua com a memòria cau del servidor. Tot i que
  representa GitHub, està guardada físicament al teu disc dur local.
  
    3. Completa: git pull = git `fetch` + git `merge`

    4. ¿Cuándo usarías fetch solo, sin pull?
  Utilitzaries només fetch quan vols comprovar de forma segura quins canvis han pujat altres 
  persones al servidor, sense arriscar-te a modificar els teus fitxers actuals ni provocar conflictes automàtics.


## Misión 6 — El conflicto (rebase de verdad)

Vamos a provocar un conflicto de verdad.

Paso 1 — dos ramas desde el mismo punto, tocando la misma línea:

#Primer he hagut de fer un push per actualitzar BITACORA.md
`git push`
Enumerando objetos: 5, listo.
Contando objetos: 100% (5/5), listo.
Compresión delta usando hasta 8 hilos
Comprimiendo objetos: 100% (3/3), listo.
Escribiendo objetos: 100% (3/3), 2.57 KiB | 2.57 MiB/s, listo.
Total 3 (delta 1), reusados 0 (delta 0), pack-reusados 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/jrodoreda-invelon/git-kata-joanrodoreda.git
   d28d6ad..d0ff36c  main -> main


`git switch main && git pull`
Ya en 'main'
Tu rama está actualizada con 'origin/main'.
Ya está actualizado.

`git switch -c feature/2-postre-tarta`
Cambiado a nueva rama 'feature/2-postre-tarta'
# en menu.md: cambia "- Fruta" por "- Tarta de queso"

`git add .
git commit -am "Change dessert to cheesecake"`
[feature/2-postre-tarta acab2ff] Change dessert to cheesecake
 1 file changed, 1 insertion(+), 1 deletion(-)

`git push -u origin feature/2-postre-tarta`
Enumerando objetos: 5, listo.
Contando objetos: 100% (5/5), listo.
Compresión delta usando hasta 8 hilos
Comprimiendo objetos: 100% (3/3), listo.
Escribiendo objetos: 100% (3/3), 329 bytes | 329.00 KiB/s, listo.
Total 3 (delta 2), reusados 0 (delta 0), pack-reusados 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: 
remote: Create a pull request for 'feature/2-postre-tarta' on GitHub by visiting:
remote:      https://github.com/jrodoreda-invelon/git-kata-joanrodoreda/pull/new/feature/2-postre-tarta
remote: 
To https://github.com/jrodoreda-invelon/git-kata-joanrodoreda.git
 * [new branch]      feature/2-postre-tarta -> feature/2-postre-tarta
rama 'feature/2-postre-tarta' configurada para rastrear 'origin/feature/2-postre-tarta'.


`git switch main`
Cambiado a rama 'main'
Tu rama está actualizada con 'origin/main'.

`git switch -c feature/3-postre-helado`
Cambiado a nueva rama 'feature/3-postre-helado'
# en menu.md: cambia "- Fruta" por "- Helado de vainilla"
