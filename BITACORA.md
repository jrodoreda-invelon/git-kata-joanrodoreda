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
