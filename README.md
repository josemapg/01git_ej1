# Ejercicio 1

Se deberá crear un repositorio y realizar una serie de operaciones desde la consola de comandos sobre el mismo para posteriormente subir el repositorio a Github.

Se deberá entregar a través del formulario de prácticas indicando la URL del repositorio. En el repositorio, deberá existir un archivo readme.md con las respuestas a las siguientes preguntas: 

* ¿Qué comando utilizaste en el paso 11? ¿Por qué?

	**Respuesta**
			
	```
	$ git reset --hard HEAD~1
	```

	El commando `reset` permite mover el puntero actual del repo a un commit dado.
	Como es el commit inmediatamente anterior, el mismo está representado con `HEAD~1` en este caso.

* ¿Qué comando o comandos utilizaste en el paso 12? ¿Por qué?

	**Respuesta** Se ejecuta un comando `reflog` para revisar las últimas acciones ejecutadas en el repositorio:

	```
	$ git reflog
	```
	
	Se analiza la salida de arriba a abajo:
	
	1. Se observa que el repo llega a e6e65d1 con motivo del reset hecho desde HEAD~1
	2. HEAD~1 es el commit inmediatamente anterior al del movimiento, en este caso, con id 9ed4cc9
	
	Se vuelve a ejecutar un `reset` con opción `--hard` para dejar el working copy área sincronizado con lo existente en el repositorio, ya que se viene de tener el estado de un commit anterior (también con `--hard`): 
		
	```
	$ git reset --hard 9ed4cc9	
	```
	
* El merge del paso 13, ¿Causó algún conflicto? ¿Por qué?

	**Respuesta** No, la salida del comando `merge` sólo nos indica que está "Already up to date"; la rama `styled` tiene como ancestro el último commit al que apunta la rama `master` en ese momento, y se ha modificado desde este punto para después hacer `commit`, conceptualmente ya estaba "absorbida".


* El merge del paso 19, ¿Causó algún conflicto? ¿Por qué? 

	**Respuesta** Sí, causó conflicto porque estamos mezclando 2 ramas que parten de un ancestro común pero que en su evolución han modificado el mismo archivo y en las mismas líneas.		
* El merge del paso 21, ¿Causó algún conflicto? ¿Por qué?

	**Respuesta**
	No causa conflicto, la historia es líneal y git realiza un merge fast-forward 

* ¿Qué comando o comandos utilizaste en el paso 25?

	**Respuesta** Utilicé un alias `graph` que había definido a partir del comando `config` como  `log --graph --decorate`. El comando `log` utilizado con los modificadores anteriores hace un diagrama de los commits de la rama actual que permite una sencilla interpretación gráfica (dependiendo de la limpieza en sí misma del repo claro está).
		
	```
	$ git graph
	```
	
* El merge del paso 26, ¿Podría ser fast forward? ¿Por qué? 

	**Respuesta** Si no se hubiese especificado el modificador `no-ff` entonces el merge hubiese sido por defecto fast-forward al no tener nada configurado en contra de esto y al tener una historia lineal desde `master` hasta la nueva rama `title`

* ¿Qué comando o comandos utilizaste en el paso 27?

	**Respuesta** Se utilizó un reset sobre `HEAD~1`, al estar en `master` git reconoce que debe ir al commit del que provenía el merge (no va  hacia el commit de la rama `title`.
		
	```
	$ git reset HEAD~1
	```

* ¿Qué comando o comandos utilizaste en el paso 28? 

	**Respuesta** Para deshacer los cambios en el working copy y dejarlos sincronizados con lo existente en el repo, he optado por usar un comando `reset` con modificador `--hard` para forzar el limpiado del working copy area y finalmente especido `HEAD~0` para quedar en el commit actual
		
	```
	$ git reset --hard HEAD~0
	```	

* ¿Qué comando o comandos utilizaste en el paso 29?

	**Respuesta** He tenido que usar `-D` para forzar el borrado, ya que el commit quedará detached al no estar mergeado con ninguna otra rama.
		
	```
	$ git branch -D title
	```	

* ¿Qué comando o comandos utilizaste en el paso 30? 

	**Respuesta** Analizando la salida de `reflog` busqué el id del commit del merge y después hice desde la rama `master` un comando `reset` con `--hard` para no mantener el working copy area y sincronizarla con el repo en ese punto
		
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...  
	$ ... 2. se detecta el commit de merge en master absorbiendo a title, con id 9fed53c ...
	$ git reset --hard 9fed53c
	```

* ¿Qué comando o comandos usaste en el paso 32?

	**Respuesta** 
	
	No se deduce si lo que queremos es mover sólo HEAD ó mover no sólo HEAD si no también master (y deshacer):
	
	*Opción 1* Movimiento de HEAD+master

	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo  
	$ ... 2. el commit inicial tiene id e6e65d1 ...
	$ git reset --hard e6e65d1
	```
	
	*Opción 2* Movimiento sólo de HEAD sin modificar el puntero master

	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...  
	$ ... 2. el commit inicial tiene id e6e65d1 ...
	$ git checkout e6e65d1
	```	

* ¿Qué comando o comandos usaste en el punto 33?

	**Respuesta**
	
	En base a la opción 1 o 2 aplicada anteriormente, se muestra la secuencia a utilizar:

	*Opción 1*		

	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...  
	$ ... 2. el commit inicial tiene id 9fed53c ...
	$ git reset --hard 9fed53c
	```

	*Opción 2*	
	
	```
	$ git checkout master
	```
 
Los pasos a ejecutar son los siguientes (los pasos en negrita indican que hay una pregunta asociada):

1. Crear un repositorio
	
	**Pasos resolución 1:**
	
	```
	$ mkdir ejercicio1
	$ cd ejercicio1
	$ git init
	```
	
	
2. Crear un archivo git-nuestro.md con el contenido: 

		Git nuestro  	
	
		Git nuestro que estas en los repos  
	
		Comprimidos sean tus commits  
	
		Venga a nosotros tu log  
	
		En el local como en el remote  
	
		Danos hoy nuestro pull de cada día  
	
		Perdona nuestros conflictos  
	
		Como también perdonamos los de otros geeks  
	
		No nos dejes caer en detached HEAD  
	
		y líbranos de SVN  
		
		git commit --amend

	**Pasos resolución 2:**

	Usar cualquier editor
	
	```
	$ nano git-nuestro.md
	$ ... añadir el texto superior al editor y guardar ...
	```


3. Añadir git-nuestro.md al staging area

	**Pasos resolución 3:**
	
	```
	$ git add git-nuestro.md
	```

4. Mover lo que hay en el staging area al repositorio

	**Pasos resolución 4:**
	
	```
	$ git commit -m "Versión 1 de git nuestro"
	```

5. Crear una rama llamada “styled”

	**Pasos resolución 5:**
	
	```
	$ git branch styled
	```

6. Listar las ramas que hay en el repositorio

	**Pasos resolución 6:**
	
	```
	$ git branch
	```

7. Moverse a la rama “styled”

	**Pasos resolución 7:**
	
	```
	$ git checkout styled
	```

8. Comprobar que se está en la rama correcta

	**Pasos resolución 8:**
	
	```
	$ git branch
	```

9. Modificar en el archivo git-nuestro.md:  

		*Git* nuestro que estas en los repos  
		
		Comprimidos sean tus *commits*  
		
		Venga a nosotros tu *log*  
		
		En el local como en el *remote*  
		
		Danos hoy nuestro *pull* de cada día
		
		Perdona nuestros *conflictos*
		
		Como también perdonamos los de otros geeks
		
		No nos dejes caer en *detached HEAD*
		
		y líbranos de *SVN*
		
		git commit --amend

	**Pasos resolución 9:**
	
	Usar cualquier editor
	
	```
	$ nano git-nuestro.md
	$ ... añadir el contenido anterior, guardar y cerrar...
	```
	
10. Añadir los cambios al staging área y luego pasarlos al repositorio

	**Pasos resolución 10:**
		
	```
	$ git add git-nuestro.md
	$ git commit -m "Versión 2 de git nuestro con markup de markdown"
	```

11. Deshacer el último commit (perdiendo los cambios realizados en el working copy) 

	**Pasos resolución 11:**
		
	```
	$ git reset --hard HEAD~1
	```

12. Rehacer el último commit (el que acabamos de deshacer)

	**Pasos resolución 12:**
		
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...	
	$ ... 2. se observa que el repo llega a e6e65d1 con motivo del reset hecho desde HEAD~1 ...
	$ ... 3. HEAD~1 es el commit inmediatamente anterior, con id 9ed4cc9
	$ git reset --hard 9ed4cc9
	```

13. Hacer un merge con ‘master’ (styled absorbe a master)

	**Pasos resolución 13:**
	
	Se parte de estar en la rama `styled`:
		
	```
	$ git merge master
	```	
	
	Este comando sólo nos indica que está "Already up to date", la rama `styled` tiene como ancestro el último commit al que apunta la rama `master` y se ha modificado desde este punto para después hacer `commit`, conceptualmente ya estaba "absorbida".

14. Volver a la rama master

	**Pasos resolución 14:**
			
	```
	$ git checkout master
	```	

15. Crear una nueva rama llamada “htmlify” 

	**Pasos resolución 15:**
			
	```
	$ git branch htmlify
	```	

16. Cambiar a la rama htmlify

	**Pasos resolución 16:**
			
	```
	$ git checkout htmlify
	```	


17. Modificar en el archivo git-nuestro.md: 
		
		<p><em>Git</em> nuestro que estas en los repos<br />
		
		Comprimidos sean tus <em>commits</em><br />
		
		Venga a nosotros tu <em>log</em><br />
		
		En el local como en el <em>remote</em><br />
		
		Danos hoy nuestro <em>pull</em> de cada día<br />
		
		Perdona nuestros <em>conflictos</em><br />
		
		Como también perdonamos los de otros geeks<br />
		
		No nos dejes caer en <em>detached HEAD</em><br />
		
		y líbranos de <em>SVN</em><br />
		
		<code>git commit --amend</code></p>

	**Pasos resolución 17:**
	
	Usar cualquier editor
	
	```
	$ nano git-nuestro.md
	$ ... añadir el contenido anterior, guardar y cerrar...
	```
	
18. Hacer un commit

	**Pasos resolución 18:**
		
	```
	$ git add git-nuestro.md
	$ git commit -m "Versión 3 de nuestro git nuestro con markup de HTML"
	```

19. Hacer un merge de “htmlify” en “styled” (styled absorbe a htmlify)

	**Pasos resolución 19:**
		
	```
	$ git checkout styled
	$ git merge htmlify
	```
	Al hacer el merge hay conflictos

20. Si hay conflictos, deberemos resolverlos quedándonos con el contenido de la rama “styled”.

	**Pasos resolución 20:**
	
	Usar cualquier editor
	
	```
	$ nano git-nuestro.md
	$ ... añadir el contenido anterior, guardar y cerrar...
	$ git add git-nuestro.md
	$ git commit
	$ ... dejar el texto por defecto, guardar y cerrar...
	```


21. Desde “master”, hacer un merge con “styled”

	**Pasos resolución 21:**
	
	```
	$ git checkout master
	$ git merge styled
	```
	No causa conflicto, la historia es línea y git realiza un merge fast-forward 
	
22. Crear una rama “title” y cambiarse a esa rama

	**Pasos resolución 22:**
	
	```
	$ git branch title
	$ git checkout title
	```
	
23. Añadir un título (a tu gusto) al archivo git-nuestro.md y hacer un commit. 

	**Pasos resolución 23:**
	
	Usar cualquier editor
	
	```
	$ nano git-nuestro.md
	$ ... añadir título, guardar y cerrar...
	$ git add git-nuestro.md
	$ git commit -m "Añadido título a git nuestro"
	$ ... dejar el texto por defecto, guardar y cerrar...
	```

24. Volver a la rama master

	**Pasos resolución 24:**
		
	```
	$ git checkout master
	```

25. Dibujar el diagrama


	**Pasos resolución 25:**
		
	```
	$ git graph
	$ ... alias definido previamente con comando config como "log --graph --decorate"
	```

26. Hacer un merge “no fast-forward” de “title” en “master” (master absorbe a title) 

	**Pasos resolución 26:**
		
	```
	$ git merge --no-ff title
	```

27. Deshacer el merge (sin perder los cambios del working copy)

	**Pasos resolución 27:**
		
	```
	$ git reset HEAD~1
	```

28. Descartar los cambios

	**Pasos resolución 28:**
		
	```
	$ git reset --hard HEAD~0
	```	

29. Eliminar la rama “title”

	**Pasos resolución 29:**
		
	```
	$ git branch -D title
	```	
	
	He tenido que usar `-D` para forzar el borrado, ya que el commit quedará detached al no estar mergeado con ninguna otra rama.


30. Rehacer el merge que hemos deshecho

	**Pasos resolución 30:**
		
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...	
	$ ... 2. se detecta el commit de merge en master absorbiendo a title, con id 9fed53c ...
	$ git reset --hard 9fed53c
	```

31. Volver a master y eliminar el resto de ramas

	**Pasos resolución 31:**
		
	En todo este tiempo no he movido la rama activa `master`
		
	```
	$ git branch -d styled
	$ git branch -d htmlify
	```
	
	Se puede utilizar el comando `-d` al estar todo el contenido mergeado.

32. Volver al commit inicial cuando se creó el poema

	**Pasos resolución 32:**

	No se deduce si lo que queremos es mover sólo HEAD ó mover no sólo HEAD si no también master:
	
	*Opción 1* Movimiento de HEAD+master
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...	
	$ ... 2. el commit inicial tiene id e6e65d1 ...
	$ git reset --hard e6e65d1
	```
	
	*Opción 2* Movimiento sólo de HEAD sin modificar el puntero master
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...	
	$ ... 2. el commit inicial tiene id e6e65d1 ...
	$ git checkout e6e65d1
	```

33. Volver al estado final, cuando pusimos título al poema

	**Pasos resolución 33:**
	
	En base a la opción 1 o 2 aplicada anteriormente, se muestra la secuencia a utilizar:

	*Opción 1*		
	```
	$ git reflog
	$ ... 1. se revisan las últimas acciones en el repo ...
	$ ... 2. el commit inicial tiene id 9fed53c ...
	$ git reset --hard 9fed53c
	```

	*Opción 2*	
	```
	$ git checkout master
	```
	
34. Crear los siguientes tags:

	**Pasos resolución 34:**
	
	En todos los casos se utiliza `reflog` para localizar los commits:

	* inicial: en el primer commit

		```
		$ git tag inicial e6e65d1
		```
	
	* styled: modificación del paso 10

		```
		$ git tag styled 9ed4cc9
		```
	
	* htmlify: modificación del paso 17-18

		```
		$ git tag htmlify 4539af1
		```
	
	* title: modificación del paso 30
	
		```
		$ git tag title 1afb42b
		```
	
35. Ir al tag htmlify

	```
	$ git checkout htmlify
	```
