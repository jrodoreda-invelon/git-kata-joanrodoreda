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

# Paso 1 — dos ramas desde el mismo punto, tocando la misma línea:

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
<<<<<<< HEAD
=======

`git commit -am "Change dessert to vanilla ice cream"`
======= ¿Qué es exactamente -am?
Hasta ahora habías usado git commit -m "mensaje", que requiere que primero hagas manualmente un git add archivo para meter los cambios en el staging area.

La bandera -am es simplemente un atajo que combina dos opciones (-a y -m):

-a (de all): Le dice a Git "haz un add automático a todos los archivos que ya estás rastreando y que he modificado".

-m (de message): Te permite escribir el mensaje del commit.

Básicamente, git commit -am "..." te ahorra escribir el git add previo en una línea separada.
(⚠️ Ojo: Solo sirve para archivos que ya existían en el repositorio y has modificado. Si creas un archivo totalmente nuevo, el -a no lo detecta y tendrás que hacer el git add a mano).

# Paso 2 — mergea SOLO la rama de la tarta (PR en GitHub → merge).

Fet

# Paso 3 — ahora actualiza la rama del helado sobre el nuevo main:

`git switch feature/3-postre-helado`
M       BITACORA.md
Ya en 'feature/3-postre-helado'
Tu rama está adelantada a 'origin/feature/3-postre-helado' por 1 commit.
  (usa "git push" para publicar tus commits locales)

`git fetch origin`
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Desempaquetando objetos: 100% (1/1), 916 bytes | 916.00 KiB/s, listo.
Desde https://github.com/jrodoreda-invelon/git-kata-joanrodoreda
   d0ff36c..707f78e  main       -> origin/main

`git rebase origin/main`
error: no se puede rebase: Tienes cambios sin marcar.
error: Por favor, confírmalos o guárdalos.

# Paso 4 — resuélvelo. Decide qué quieres que quede (puedes dejar los dos postres), borra las tres líneas de marcadores (<<<<<<<, =======, >>>>>>>) y:

$ git commit -am "Change dessert to vanilla ice cream"
# He tenido que hacer commit para guardar menu.md para que me apareciese el siguiente Conflicto

git add menu.md
git rebase --continue
git push --force-with-lease

¿Qué significa HEAD en el conflicto? ¿Tu cambio o el de main? (⚠️ en un rebase no es lo que crees)
- En un rebase, la perspectiva se invierte. HEAD es la rama base sobre la que te estás aplicando (en este caso, main 
  con la tarta de queso), mientras que tus propios cambios (el helado) son los que intentan inyectars.
¿Por qué el push normal falló y necesitó --force-with-lease?
- Porque al hacer rebase reescribiste la historia de tu rama local. Como los commits cambiaron,
  Git bloquea un push normal para evitar que sobrescribas el servidor por accidente.
¿Qué diferencia hay entre --force y --force-with-lease? ¿Por qué usamos el segundo?
- --force sobrescribe el servidor ciegamente y puede borrar el trabajo de otros. --force-with-lease es un seguro de 
  vida: antes de sobrescribir, comprueba que nadie más haya modificado esa rama remota mientras tú arreglabas el conflicto.
Si te hubieras arrepentido a mitad del rebase, ¿cómo lo cancelas? (pista: git rebase --?????)
- Usando el comando `git rebase --abort`.

## Misión 7 — merge vs rebase, visto en el grafo

`$ git log --oneline --graph --all`
* 285f733 (HEAD -> feature/3-postre-helado, origin/feature/3-postre-helado) Actualizar notas en la bitácora
* 1988faa Change dessert to vanilla ice cream
* 55b3672 Change dessert to vanilla ice cream
* 6cbbe1e Change dessert to vanilla ice cream
*   707f78e (origin/main, origin/HEAD) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
|\  
| * acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
|/  
* d0ff36c (main) Guardamos BITACORA.md
* d28d6ad Update README with Mission 5 details
*   8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
|\  
| * 0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
| * 7081f8d Add starters section to the daily menu
|/  
* 00366e4 Add base menu
* dbc4172 Initial commit

# Dibuja (o pega) el grafo. ¿Qué forma tiene la historia de la rama que mergeaste y cuál la de la que rebaseaste?
Rama con Merge (Tarta): Deja una estructura de bucle o bifurcación con un nudo de unión (|\ y |/). El commit de merge 
actúa como un puente que conecta las dos historias paralelas, preservando la cronología real de cuándo 
se desvió y se unió el código.

Rama con Rebase (Helado): Deja una línea completamente recta y secuencial. El rebase "reubica" tus cambios desde la 
base antigua hasta la punta actual de main, reescribiendo la historia para que parezca que siempre se trabajó de 
forma lineal y ordenada.

#  La regla de oro corporativa (¿Por qué este orden?)
Rebase para actualizarte tú de manera local: Te permite limpiar tu historial y ponerte al día con respecto a los 
cambios de tus compis sin ensucian el grafo con merges inútiles de sincronización.

Merge (vía PR) para integrar a producción (main): Deja constancia oficial, trazable y sujeta a revisión en el 
servidor de que esa funcionalidad ha sido validada y aprobada para entrar en el tronco principal.

# Regla de la casa: rebase para actualizar tu rama, merge (vía PR) para integrar a main. ¿Por qué en ese orden?

Porque combinamos lo mejor de ambos mundos:

- Usas rebase de forma local en tu rama de trabajo para ponerte al día con respecto a main de forma limpia y sin 
desordenar el grafo con merges innecesarios.

- Usas merge (vía PR) en el servidor para integrar tus cambios en main, lo que deja una traza oficial, auditable 
y aprobada de que esa funcionalidad ha entrado a producción.

# ¿Por qué NUNCA debes hacer rebase de una rama que comparten otras personas?

Porque el rebase reescribe la historia y cambia los hashes (identificadores) de los commits. Si haces rebase de una 
rama compartida (como una feature branch donde trabaja otro compañero), su ordenador y el servidor se quedarán 
desincronizados con una historia inventada y divergente, destruyendo su trabajo y obligándole a hacer malabares 
técnicos para recuperar sus cambios. El rebase solo se tolera en ramas estrictamente personales y locales.

## Misión 8 — Deshacer (revert, reset, reflog)

Primero, crea una rama limpia para hacer las pruebas:

git switch main && git pull
git switch -c sandbox/deshacer

# 8.1 — revert (seguro: crea un commit nuevo)
echo "- Pizza con piña" >> menu.md
git commit -am "Add controversial pizza"

git revert HEAD          # se abre el editor, guarda y cierra
git log --oneline

cbf9e94 (HEAD -> sandbox/deshacer) Revert "Add controversial pizza"
f391eaa Add controversial pizza
707f78e (origin/main, origin/HEAD, main) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
d0ff36c Guardamos BITACORA.md
d28d6ad Update README with Mission 5 details
8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
7081f8d Add starters section to the daily menu
00366e4 Add base menu
dbc4172 Initial commit

→ ¿Desapareció el commit malo o hay dos commits ahora?
- Hay dos commits ahora. El commit malo no desaparece de la historia; lo que hace git revert es añadir un nuevo 
  commit al final que aplica los cambios inversos para anular el anterior, dejando constancia de todo el historial.

# 8.2 — reset --soft (deshace el commit, guarda los cambios)
echo "- Otro error" >> menu.md
git commit -am "Bad commit"

git reset --soft HEAD~1
git status

→ ¿Dónde están tus cambios: en staging, sin stagear, o perdidos?
- Están en el área de preparación (staging). El commit desaparece, pero los archivos modificados se quedan 
  automáticamente guardados y preparados (staged), listos para volver a commitearlos o modificarlos.

# 8.3 — reset --hard (destruye) + reflog (rescata)
git log --oneline          # APUNTA EL HASH DE ARRIBA, lo vas a necesitar

git reset --hard HEAD~1
git log --oneline          # ya no está

git reflog                 # 🪄 aquí sigue TODO
git reset --hard <el-hash-que-apuntaste>
git log --oneline          # ha vuelto

`git log --oneline`
cbf9e94 (HEAD -> sandbox/deshacer) Revert "Add controversial pizza"
f391eaa Add controversial pizza
707f78e (origin/main, origin/HEAD, main) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
d0ff36c Guardamos BITACORA.md
d28d6ad Update README with Mission 5 details
8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
7081f8d Add starters section to the daily menu
00366e4 Add base menu
dbc4172 Initial commit

`git reset --hard HEAD~1` 
HEAD está ahora en f391eaa Add controversial pizza

# Ya no está 
`git log --oneline`  
f391eaa (HEAD -> sandbox/deshacer) Add controversial pizza
707f78e (origin/main, origin/HEAD, main) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
d0ff36c Guardamos BITACORA.md
d28d6ad Update README with Mission 5 details
8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
7081f8d Add starters section to the daily menu
00366e4 Add base menu
dbc4172 Initial commit

`git reflog`
Sigue todo igual pero añade a la visión absosutamente todos los cambios que se han hecho hasta el momento:
f391eaa (HEAD -> sandbox/deshacer) HEAD@{0}: reset: moving to HEAD~1
cbf9e94 HEAD@{1}: reset: moving to HEAD~1
79226ab HEAD@{2}: commit: Bad commit
cbf9e94 HEAD@{3}: revert: Revert "Add controversial pizza"
f391eaa (HEAD -> sandbox/deshacer) HEAD@{4}: commit: Add controversial pizza
707f78e (origin/main, origin/HEAD, main) HEAD@{5}: checkout: moving from main to sandbox/deshacer
707f78e (origin/main, origin/HEAD, main) HEAD@{6}: pull: Fast-forward
d0ff36c HEAD@{7}: checkout: moving from feature/3-postre-helado to main
9862054 (origin/feature/3-postre-helado, feature/3-postre-helado) HEAD@{8}: commit: Misión 7 done
285f733 HEAD@{9}: rebase (finish): returning to refs/heads/feature/3-postre-helado
285f733 HEAD@{10}: rebase (continue): Actualizar notas en la bitácora
1988faa HEAD@{11}: rebase (continue): Change dessert to vanilla ice cream
55b3672 HEAD@{12}: rebase (continue): Change dessert to vanilla ice cream
6cbbe1e HEAD@{13}: rebase (pick): Change dessert to vanilla ice cream
707f78e (origin/main, origin/HEAD, main) HEAD@{14}: rebase (start): checkout origin/main
af46011 HEAD@{15}: commit: Actualizar notas en la bitácora
ba639fa HEAD@{16}: checkout: moving from feature/3-postre-helado to feature/3-postre-helado
ba639fa HEAD@{17}: commit: Change dessert to vanilla ice cream
9570287 HEAD@{18}: commit: Change dessert to vanilla ice cream
18b4e9a HEAD@{19}: commit: Change dessert to vanilla ice cream
d0ff36c HEAD@{20}: checkout: moving from main to feature/3-postre-helado
d0ff36c HEAD@{21}: checkout: moving from feature/2-postre-tarta to main
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) HEAD@{22}: commit: Change dessert to cheesecake
d0ff36c HEAD@{23}: checkout: moving from main to feature/2-postre-tarta
d0ff36c HEAD@{24}: checkout: moving from main to main
d0ff36c HEAD@{25}: checkout: moving from main to main
d0ff36c HEAD@{26}: commit: Guardamos BITACORA.md
d28d6ad HEAD@{27}: checkout: moving from main to main
d28d6ad HEAD@{28}: pull: Fast-forward
8422ee0 HEAD@{29}: pull: Fast-forward
00366e4 HEAD@{30}: checkout: moving from feature/1-primeros-platos to main
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) HEAD@{31}: commit: Antes de Mision 4
7081f8d HEAD@{32}: rebase (finish): returning to refs/heads/feature/1-primeros-platos
7081f8d HEAD@{33}: rebase (squash): Add starters section to the daily menu
dd5a667 HEAD@{34}: rebase (squash): # Esta es una combinación de 7 commits.
667e743 HEAD@{35}: rebase (squash): # Esta es una combinación de 6 commits.
e0b44f7 HEAD@{36}: rebase (squash): # Esta es una combinación de 5 commits.
8fa689a HEAD@{37}: rebase (squash): # Esta es una combinación de 4 commits.
acccc06 HEAD@{38}: rebase (squash): # Esta es una combinación de 3 commits.
26c6ad9 HEAD@{39}: rebase (squash): # Esta es una combinación de 2 commits.
fff3d2c HEAD@{40}: rebase (start): checkout main
a8725c3 HEAD@{41}: rebase (finish): returning to refs/heads/feature/1-primeros-platos
a8725c3 HEAD@{42}: rebase (start): checkout main
a8725c3 HEAD@{43}: rebase (finish): returning to refs/heads/feature/1-primeros-platos
a8725c3 HEAD@{44}: rebase (start): checkout main
a8725c3 HEAD@{45}: rebase (abort): returning to refs/heads/feature/1-primeros-platos
00366e4 HEAD@{46}: rebase (start): checkout main
a8725c3 HEAD@{47}: commit: Previo a Mission3
ed58bbe HEAD@{48}: commit: BITACORA.md
000e696 HEAD@{49}: commit (amend): Add pumpkin cream to starters
9513bd5 HEAD@{50}: commit: asdf
d09fd3d HEAD@{51}: commit: otra vez
3872702 HEAD@{52}: commit: fix
59cb197 HEAD@{53}: commit: wip
c267fc5 HEAD@{54}: commit: vuelta a inicio
fff3d2c HEAD@{55}: commit: wip
00366e4 HEAD@{56}: checkout: moving from main to feature/1-primeros-platos
00366e4 HEAD@{57}: commit: Add base menu
dbc4172 HEAD@{58}: clone: from https://github.com/jrodoreda-invelon/git-kata-joanrodoreda.git

`git reset --hard cbf9e94` o `git reset --hard HEAD@{1}`
HEAD está ahora en cbf9e94 Revert "Add controversial pizza"

-- Respuestas a las preguntas --
- revert vs reset: ¿cuál usas en una rama compartida y por qué?
  - Se usa git revert en ramas compartidas (como main). Esto se debe a que revert crea un nuevo commit que deshace los
    cambios anteriores sin alterar la historia previa. 
  - Si usaras reset en una rama pública, reescribirías la historia y romperías el repositorio y el trabajo de tus compañeros.

- --soft vs --mixed vs --hard: una frase cada uno.
  - --soft deshace el commit pero deja todos tus cambios intactos en el área de preparación (staging).
  - --mixed (por defecto) deshace el commit y saca los cambios del staging, pero los mantiene en tus ficheros locales.
  - --hard es destructivo: deshace el commit y borra por completo los cambios de tus archivos, volviendo exactamente al 
    estado de ese commit.

- ¿QUÉ ES EL STAGING?
  - Para entenderlo bien, Git divide tus archivos en tres zonas o estados:
  1. `El directorio de trabajo (Working Directory):` Es tu PyCharm o tu carpeta local. Aquí es donde abres los archivos,
     escribes código, borras líneas o pruebas cosas. Los cambios aquí son libres y caóticos; Git sabe que los has 
     tocado, pero aún no les ha hecho "una foto oficial".
  2. `El área de preparación (Staging Area / Index):` Es la antesala. Cuando ejecutas un git add <archivo>, estás metiendo
     ese archivo en la caja de cartón (el staging). Le estás diciendo a Git: "De todo lo que he modificado hoy, solo 
     esto de aquí es lo que quiero que forme parte del próximo paquete/commit".
  3. `El repositorio local (Repository / Historial de commits):` Es cuando ya cierras la caja, le pones la cinta adhesiva,
     le escribes un título (git commit -m "...") y la guardas en tu estantería. Ese paquete ya forma oficial parte de 
     la historia inmutable de tu proyecto.

- ¿Qué es el reflog y cuánto tiempo guarda las cosas?
  - El reflog es un registro interno y oculto que anota cada movimiento de la etiqueta HEAD en tu repositorio local.
    Por defecto, almacena las referencias durante 90 días antes de permitir su limpieza definitiva.

- Pregunta de verdad: ¿existe algo en Git que pierdas para siempre tras un commit?
  - `No`. Cualquier cambio que haya llegado a ser registrado mediante un commit (aunque se haya hecho un reset --hard o 
    se hayan borrado ramas) se queda guardado en la base de datos interna de Git y puede recuperarse consultando el git
    reflog y su respectivo hash. Solo se pierde aquello que nunca llegó a hacerse add ni commit.


## 🎒 Misión 9 — stash: la interrupción

Estás a medias de algo y te piden arreglar un bug ya.

git switch -c feature/4-bebidas
echo "## Bebidas" >> menu.md
echo "- Agua" >> menu.md
# NO commitees

git switch main            # 💢 Git te deja o te bloquea?
# No me deja, aparece:
error: Los cambios locales de los siguientes archivos serán sobrescritos por checkout:
        menu.md
Por favor realice un commit con los cambios o un stash antes de cambiar ramas.

git stash
git status                 # limpio
git switch main
# ...arreglarías el bug aquí...
git switch feature/4-bebidas
git stash pop
git status                 # volvieron tus cambios

-- Respuestas a las preguntas --

- ¿Qué pasó exactamente al intentar cambiar de rama sin commitear?
  - Git bloqueó el cambio de rama y lanzó un error. Lo hace como medida de seguridad para evitar que los cambios
    locales actuales se mezcle o se pierdan al saltar a otro contexto.
- git stash pop vs git stash apply: ¿diferencia?
  - git stash pop saca los cambios guardados del cajón temporal (stash) y los aplica en tu archivo, borrándolos del
    stash. git stash apply también los aplica, pero los mantiene guardados en el stash por si quieres reutilizarlos o 
    aplicarlos en otro sitio.
- ¿Dónde se guarda lo del stash?
  - Se guarda en el directorio local .git/refs/stash de tu repositorio como un objeto de commit independiente dentro de 
    la base de datos interna de Git.


## Misión 10 — cherry-pick (Un commit suelto)

Te interesa un commit de otra rama, no la rama entera.

`git log --oneline feature/2-postre-tarta`    # copia el hash del commit de la tarta
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
d0ff36c Guardamos BITACORA.md
d28d6ad Update README with Mission 5 details
8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
7081f8d Add starters section to the daily menu
00366e4 Add base menu
dbc4172 Initial commit

`git switch -c sandbox/cherry main`
`git cherry-pick <hash>` cambiamos <hash> por acab2ff que es identificador del commit de la tarta
# me indica que quiere no hay nada para lo que hacer commit

`git log --oneline`
707f78e (HEAD -> sandbox/cherry, origin/main, origin/HEAD, main) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
d0ff36c Guardamos BITACORA.md
d28d6ad Update README with Mission 5 details
8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
7081f8d Add starters section to the daily menu
00366e4 Add base menu
dbc4172 Initial commit

# entonces he ejecutado hit cherry-pick --abort

-- Respuestas a las preguntas --

- ¿El hash del commit cherry-pickeado es el mismo que el original? ¿Por qué?
  - No, es diferente. Aunque el contenido y los cambios de código sean idénticos, al aplicar un cherry-pick en otra 
    rama, Git genera un nuevo commit con una nueva marca de tiempo, un nuevo padre y, por tanto, un hash totalmente nuevo.
- ¿Cuándo usarías cherry-pick en vez de merge?
  - Se usa cuando necesitas traerte un arreglo puntual o hotfix (por ejemplo, corregir un bug crítico que está en una 
    rama de desarrollo) hacia producción (main) de forma inmediata, sin necesidad de fusionar toda la rama entera ni 
    traerte cambios inacabados.


## Misión 11 — .gitignore

echo "SECRET_KEY=no-subas-esto" > .env
git status                 # ¿aparece .env?

echo ".env"   >> .gitignore
echo "venv/"  >> .gitignore
`git status`           # ¿y ahora?
En la rama feature/4-bebidas
Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
        modificados:     .gitignore

sin cambios agregados al commit (usa "git add" y/o "git commit -a")

git add .gitignore && git commit -m "Add gitignore rules"

-- Respuestas a las preguntas --

- ¿Por qué .env nunca va al repo?
  - Porque contiene información confidencial, credenciales privadas, claves de API (secret keys) y contraseñas de
    entornos locales. Subirlas al repositorio público o compartido expondría datos críticos de seguridad.
- La trampa: si un fichero YA está trackeado, añadirlo al .gitignore no lo saca. ¿Qué comando hace falta?
  - git rm --cached <nombre-archivo>
- Si subes una contraseña por error y luego haces un commit borrándola, ¿está a salvo? ¿Por qué no?
  - No está a salvo. Aunque en el commit actual ya no aparezca, la contraseña sigue grabada y accesible para cualquier
    persona en el historial de commits anteriores de Git. Para eliminarla por completo habría que reescribir la 
    historia del repositorio o revocar/cambiar la contraseña inmediatamente en el servicio afectado.


## Misión 12 — Investigar

`git log --oneline --graph --all --decorate`
* a04011f (HEAD -> feature/4-bebidas) Actualizar notas de la bitácora
* 837fe5d Actualizar notas de la bitácora
* 29712bd Add gitignore rules
* 8f351ca Actualizar notas de la bitácora
* add1cb4 Actualizar notas de la bitácora
* dbde2d8 Actualizar notas de la bitácora
* 35b4cb0 Actualizar notas de la bitácora
* edfe015 Mision 9 done
* 6611399 Actualizar notas de la bitácora
* 84f4f64 Actualizar notas de la bitácora
| * a2918c5 (refs/stash) WIP on feature/4-bebidas: 41c0579 Actualizar notas de la bitácora
|/| 
| * 75451a4 index on feature/4-bebidas: 41c0579 Actualizar notas de la bitácora
|/  
* 41c0579 Actualizar notas de la bitácora
* 1b9902d (sandbox/deshacer) Revert "Add controversial pizza"
* cc3b0d8 Add controversial pizza
* f391eaa Add controversial pizza
| * 9862054 (origin/feature/3-postre-helado, feature/3-postre-helado) Misión 7 done
| * 285f733 Actualizar notas en la bitácora
| * 1988faa Change dessert to vanilla ice cream
| * 55b3672 Change dessert to vanilla ice cream
| * 6cbbe1e Change dessert to vanilla ice cream
|/  
*   707f78e (origin/main, origin/HEAD, sandbox/cherry, main) Merge pull request #2 from jrodoreda-invelon/feature/2-postre-tarta
|\  
| * acab2ff (origin/feature/2-postre-tarta, feature/2-postre-tarta) Change dessert to cheesecake
|/  
* d0ff36c Guardamos BITACORA.md
* d28d6ad Update README with Mission 5 details
*   8422ee0 Merge pull request #1 from jrodoreda-invelon/feature/1-primeros-platos
|\  
| * 0682717 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Antes de Mision 4
| * 7081f8d Add starters section to the daily menu
|/  
* 00366e4 Add base menu
* dbc4172 Initial commit

- `Para qué se usa:` Para visualizar de forma gráfica, compacta y esquemática la estructura completa de todo el árbol
  del repositorio, mostrando todas las ramas, commits y etiquetas de un solo vistazo.

`git show <hash>`
- `Para qué se usa:` Para qué se usa: Para inspeccionar en detalle el contenido exacto, los metadatos y las líneas de 
  código concretas que se modificaron en un commit específico.

`git diff main..feature/2-postre-tarta`
- `Para qué se usa:` Para comparar línea por línea las diferencias exactas de código que existen entre dos ramas antes 
  de proceder a integrarlas o fusionarlas.

`git blame menu.md`
00366e41 (Joan 2026-10-08 13:00:27 +0200  1) # Menú del día
00366e41 (Joan 2026-10-08 13:00:27 +0200  2) 
00366e41 (Joan 2026-10-08 13:00:27 +0200  3) ## Primero
00366e41 (Joan 2026-10-08 13:00:27 +0200  4) - Sopa
7081f8da (Joan 2026-10-09 09:52:29 +0200  5) - Ensalada
7081f8da (Joan 2026-10-09 09:52:29 +0200  6) - Gazpacho
7081f8da (Joan 2026-10-09 09:52:29 +0200  7) - Crema de calabaza
00366e41 (Joan 2026-10-08 13:00:27 +0200  8) 
00366e41 (Joan 2026-10-08 13:00:27 +0200  9) ## Segundo
00366e41 (Joan 2026-10-08 13:00:27 +0200 10) - Pollo
00366e41 (Joan 2026-10-08 13:00:27 +0200 11) 
00366e41 (Joan 2026-10-08 13:00:27 +0200 12) ## Postre
acab2ffe (Joan 2026-10-09 12:34:11 +0200 13) - Tarta de queso
7081f8da (Joan 2026-10-09 09:52:29 +0200 14) - Crema de calabaza (vegana)
f391eaad (Joan 2026-10-13 10:18:04 +0200 15) - Pizza con piña
66113998 (Joan 2026-10-13 11:32:17 +0200 16) ## Bebidas
66113998 (Joan 2026-10-13 11:32:17 +0200 17) - Agua

- `Para qué se usa:` Para auditar un archivo línea por línea y averiguar exactamente qué usuario modificó cada 
  fragmento de código y en qué commit lo hizo.

`git log -p menu.md`
- `Para qué se usa:` Para consultar el historial cronológico detallado de todos los cambios aplicados exclusivamente a
  un archivo concreto a lo largo del tiempo

  
## Misión 13 — Tag y Release

git switch main && git pull

git tag -a v1.0.0 -m "Git kata completed"
git push origin v1.0.0

`GitHub → Releases → Draft a new release → tag v1.0.0 → notas.`