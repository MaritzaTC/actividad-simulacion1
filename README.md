# Actividad de seguimiento - Simulación 1

|Integrante|correo|usuario github|
|---|---|---|
|Maritza Tabarez Cárdenas|maritza.tabarezc@udea.edu.co|MaritzaTC|
|Nombre completo integrante 2|correo integrante 2|gihub user integrante 2|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:
* Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
* Como la entrega se debe hacer modificando el archivo README, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
* Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.
   
   <details>
   <summary>Answer</summary>
    Cuando usamos el comando:   
      
   `./process-run.py -l 5:100,5:100`
   <br> 
   ![2](https://github.com/user-attachments/assets/25f6ce3c-12b8-4291-bddd-7acbeb49e5b5)

   #### Nos muestra que en el Process 0 (5:100) tiene 5 instrucciones y 100% de las instrucciones usarán la CPU (no habrá I/O), y en el segundo proceso pasa igual.
   ### ¿Cuál debería ser la utilización de la CPU?

   Los procesos están configurados para usar solo la CPU en todas sus instrucciones, sin realizar operaciones de I/O, lo que garantiza que la utilización de la CPU sea del 100%, ya 
   que no hay tiempo perdido esperando a que se completen operaciones de entrada/salida, y la CPU está constantemente ocupada ejecutando instrucciones.
   
   <br>
   
   ### ¿Por qué sabes esto? 
   Según la opción l en el script `process-run.py` 
   `parser.add_option('-l', '--processlist', default='', help='a comma-separated list of processes to run, in the form X1:Y1,X2:Y2,... where X is the number of instructions that         process should run, and Y the chances (from 0 to 100) that an instruction will use the CPU or issue an IO (i.e., if Y is 100, a process will ONLY use the CPU and issue no 
    I/Os;      if Y is 0, a process will only issue I/Os)', action='store', type='string', dest='process_list')
   parser.add_option('-L', '--iolength', default=5, help='how long an IO takes', action='store', type='int', dest='io_length')`

   En el primer proceso, sabemos que tiene 5 instrucciones y el 100% de ellas utilizarán la CPU, sin realizar operaciones de I/O, y lo mismo ocurre con el segundo proceso, que         también tiene 5 instrucciones y el 100% de ellas usan la CPU sin realizar I/O. Esto asegura que ambos procesos estén ocupando la CPU de manera continua, lo que resulta en una    
   utilización del 100% de la CPU.

   ## ¿Los resultados de la simulación confirmaron esto?
   ✅ Cuando usamos el comando:   
      
   `./process-run.py -l 5:100,5:100 -c -p`
   <br>
   
    ![3](https://github.com/user-attachments/assets/10d504f6-8243-4e8e-bf6d-84cd5fdb56e5)
   
    Con el valor de **CPU Busy**, podemos confirmar que la CPU estuvo ocupada todo el tiempo y nunca inactiva, ya que el tiempo registrado como ocupado coincide con el tiempo total       de      la simulación, lo que indica que no hubo períodos sin actividad en la CPU.
   <br>
   

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right. 
   
   <details>
   <summary>Answer</summary>
   Al ejecutar el comando tenemos: 
   <br> 
      
   ![4](https://github.com/user-attachments/assets/9a51adb0-e88d-4fd1-93c9-4e2618859955)
   
   ## ¿Cuánto tiempo tarda en completarse ambos procesos?
   - Process 0: Usará la CPU en todas su instrucciones, comenzará a ejecutarse y terminará después de 4 unidades de tiempo.
   - Process 1: Empezará con una operación de I/O (que lo pondrá en estado bloqueado), luego se desbloqueará cuando la I/O termine y ejecutará la instrucción io_done, lo que suma 1 
   unidad adicional.
   Por lo tanto serán **5** unidades de tiempo.

   ## ¿Los resultados de la simulación confirmaron esto? 
   ❎ Cuando usamos el comando:
   
   <br>
   
   ![5](https://github.com/user-attachments/assets/9e917a32-4bc1-498b-9b42-75d5b2a1689f)
     - Process 0: Comenzó a ejecutarse y ocupó la CPU durante 4 unidades de tiempo, como esperábamos.
     - Process 1: Emitió una operación de I/O en el tiempo 5. Este proceso se bloqueó después de emitir la I/O, ya que no se puede ejecutar hasta que la operación de I/O       
     termine.
  
   <br>
   
   El proceso permaneció bloqueado [6-10] hasta que la I/O se completó.

   
    Finalmente, en el tiempo 11, el proceso 1 completó la operación de I/O y ejecutó la instrucción io_done, terminando así su ejecución.
    Por lo tanto, el tiempo de ejecución de ambos procesos es de  **11** unidades de tiempo.
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)
   
   <details>
   <summary>Answer</summary>
   Al ejecutar el comando tenemos: 
   <br> 

   ![6](https://github.com/user-attachments/assets/e8dbdeee-2b24-4f2b-a91f-585eac7c6f5e)

   ##  ¿Qué pasa ahora?
    - Tiempo 1: El Proceso 0 comienza y emite la operación de I/O. Esto significa que se bloqueará inmediatamente y esperará a que se complete la I/O, la CPU no es utilizada por el proceso 0 en este momento.
    - Tiempo 2-5: La CPU ahora está libre, por lo que el proceso 1 comienza a ejecutarse, donde usa la CPU para sus 4 instrucciones que requieren CPU.
    - Tiempo 6: Después de que el proceso 1 termine, el Proceso 0 podrá continuar y completar su operación de I/O (ahora se marca como io_done).
     
   ## ¿Los resultados de la simulación confirmaron esto? 
   ❎ Cuando usamos el comando:
   <br>
   ![7](https://github.com/user-attachments/assets/ce1d5564-04d4-4ddd-a6bf-964b651ab52b)

   <br> 
   
   ##  ¿Qué pasa ahora?
   
   - Tiempo 1: El Proceso 0 comienza y emite la operación de I/O. Esto significa que se bloqueará inmediatamente y esperará a que se complete la I/O, la CPU no es utilizada por el proceso 0 en este momento.
   - Tiempo 2-5: La CPU ahora está libre, por lo que el proceso 1 comienza a ejecutarse, donde usa la CPU para sus 4 instrucciones que requieren CPU.
   - Tiempo 6: El proceso 1 termina sus instrucciones, y el proceso 0 todavía está bloqueado, esperando a que su I/O termine.
   - Tiempo 7: Después de que el proceso 1 termine, el Proceso 0 podrá continuar y completar su operación de I/O (ahora se marca como io_done).
   
    ## ¿Importa cambiar el orden? ¿Por qué?
    El orden sí influye, porque determina cómo se manejan las operaciones de CPU e I/O, afectando el tiempo total de ejecución, la eficiencia de la CPU y el tiempo de espera de I/O.

   ![8](https://github.com/user-attachments/assets/603113af-0764-46b8-9e39-35cf8b51beb3)


   ### Orden de ejecución de procesos
    - El sistema no ejecuta todos los procesos simultáneamente, sino que va cambiando entre ellos.
    - En el primer caso, el proceso de I/O tiene que esperar a que el proceso de CPU termine, esto lleva más tiempo debido a la secuencia de ejecución.
    - En el segundo caso, el primer proceso se bloquea rápidamente al hacer I/O, y luego la CPU se usa más intensivamente para el segundo proceso. 

   ### Uso de la CPU e I/O
    - En el segundo caso, debido a que el proceso de I/O se ejecuta primero, se termina más rápido, lo que permite que la CPU se use casi de manera continua durante el resto del tiempo. 
    - En cambio, en el primer caso, la CPU no se utiliza de manera tan eficiente porque el primer proceso consume mucha CPU antes de que el segundo proceso haga I/O.
  
   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?
   
   <details>
   <summary>Answer</summary>
   Al ejecutar el comando tenemos: 
   <br> 
      
   ![9](https://github.com/user-attachments/assets/f4d51869-c750-4990-851d-5bb4a8960518)

   ##  ¿Qué pasa ahora?
   - Durante el primer ciclo (1), el proceso 0 está ejecutando I/O (RUN:io).
   - Luego, debido al flag SWITCH_ON_END, el proceso 0 permanece bloqueado durante todo el tiempo que esté realizando la operación de I/O.
   - El proceso 1 no comienza hasta que el proceso 0 haya terminado todo su trabajo de I/O, lo que puede ser una razón por la que el CPU no se utiliza en ese tiempo.
   - Una vez que el proceso 0 ha completado la operación de I/O, el proceso 1 comienza a ejecutar sus instrucciones de CPU.

   ## Estadísticas finales

    ![10](https://github.com/user-attachments/assets/58379370-3d16-4ad4-8800-00a1df56e02e)


   ## Conclusión
   El flag SWITCH_ON_END asegura que el sistema no cambie a otro proceso mientras uno está esperando o realizando una operación de I/O, esto implica que el proceso que está            realizando I/O ocupa toda la CPU (en términos de tiempo de espera) mientras su operación de I/O no haya terminado. Después de que el proceso de I/O termine, el sistema cambia al 
   siguiente proceso, que en este caso es el proceso 1, el cual realiza trabajo de CPU.

   Este comportamiento es reflejado en las estadísticas, donde la CPU está ocupada por el proceso 1 después de que el proceso 0 haya terminado su I/O.

   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.
   
   <details>
   <summary>Answer</summary>
    Al ejecutar el comando tenemos: 
   <br> 
      
      ![11](https://github.com/user-attachments/assets/559783c4-0863-4622-8ae4-14d62cf9a72b)
   ##  ¿Qué pasa ahora?
   - SWITCH_ON_IO provoca que el sistema cambie de proceso tan pronto como un proceso esté esperando por I/O. Por lo tanto, mientras el Proceso 0 está esperando por I/O, el sistema 
   cambia al Proceso 1 y le da tiempo para ejecutar su trabajo de CPU.
   - Proceso 1 realiza su trabajo de CPU mientras el proceso 0 está bloqueado esperando la I/O.
   - Después de que proceso 1 termina, el sistema retoma Proceso 0, que ya ha terminado su operación de I/O.
  
    ##  ¿Qué pasa ahora usando -p ?

   ![12](https://github.com/user-attachments/assets/0aa30033-51e3-40c9-b98d-9a129aa527ab)
   - CPU Busy: 6 ciclos (85.71%). Esto indica que la CPU estuvo ocupada durante casi todo el tiempo, mientras que proceso 1 estaba ejecutándose.
   - IO Busy: 5 ciclos (71.43%). Esto refleja el tiempo que el proceso 0 estuvo esperando la operación de I/O.
   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?
   
   <details>
   <summary>Answer</summary>
   Al ejecutar el comando tenemos: 
   <br> 
      
   ![13](https://github.com/user-attachments/assets/39fa6158-c3f2-401b-8ad2-82de6aff65b1)

   ## ¿Qué sucede al ejecutar esta combinación de procesos?
   Lo primero será conocer los procesos involucradros, tenemos:
   - Proceso 0: I/O que no se ejecuta inmediatamente después de completar, debido a la configuración IO_RUN_LATER.
   - Procesos 1, 2 y 3: Realizan trabajo de CPU, usando las instrucciones especificadas.

   ###  Comportamiento de cambio de proceso (-S SWITCH_ON_IO)
   El sistema cambia de proceso cada vez que uno de ellos está esperando realizar I/O, lo que permite que otro proceso use la CPU mientras uno está bloqueado esperando la 
   finalización de una operación de I/O.

   ### Comportamiento cuando I/O finaliza (-I IO_RUN_LATER)
   Aunque un proceso termina su I/O, no se ejecuta inmediatamente, lo que significa que el sistema continúa ejecutando el proceso que está usando la CPU hasta que termine.

   ### Utilización de los recursos
   - El 67.74% del tiempo la CPU está ocupada. Esto indica que la CPU está en uso, pero no de manera continua, ya que hay períodos en los que los procesos están bloqueados          esperando I/O o esperando para ejecutarse después de completar I/O.
   - El 48.39% del tiempo está ocupado realizando I/O. Esto sugiere que, aunque la CPU se usa bastante, también hay una cantidad significativa de tiempo dedicado a las operaciones       de I/O, pero el sistema no está optimizando completamente el uso de los recursos de CPU mientras los procesos esperan la finalización de las operaciones de I/O.

    ## ¿Se utilizan eficazmente los recursos del sistema?
   No completamente. Aunque la CPU se utiliza una buena parte del tiempo (67.74%), no está siendo utilizada de manera continua y eficiente debido a la configuración IO_RUN_LATER. Los procesos que están esperando I/O no se ejecutan inmediatamente cuando finaliza la I/O, lo que reduce la eficiencia en el uso de la CPU. Además, los procesos de I/O no se ejecutan de inmediato y, por lo tanto, se generan períodos de inactividad de la CPU que podrían haberse aprovechado mejor si el proceso que terminó su I/O se hubiera ejecutado de inmediato.
   </details>
   <br>

9. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?
   
   <details>
   <summary>Answer</summary>
    Al ejecutar el comando tenemos: 
   <br> 
      
      ![14](https://github.com/user-attachments/assets/fdc8a224-9479-4428-9108-6807e8bc3e10)
   ## ¿En qué se diferencia este comportamiento?
   Cuando se ejecuta el comando con la opción `-I IO_RUN_IMMEDIATE`, el comportamiento de la simulación cambia significativamente en comparación con `IO_RUN_LATER`.
   - Esto se debe a que con  `IO_RUN_LATER` después de que un proceso realiza una operación de I/O, el sistema no lo ejecuta inmediatamente, lo que significa que la CPU puede 
    quedar inactiva mientras espera que otro proceso termine o esté listo para ejecutarse y también el proceso que completó la I/O debe esperar su turno para ejecutarse, lo que 
     puede generar períodos de inactividad en la CPU.
    - En cambio con `-I IO_RUN_IMMEDIATE` tan pronto como un proceso termina una operación de I/O, el sistema lo ejecuta inmediatamente (si está listo), lo que reduce el tiempo de 
     espera y maximiza la utilización de la CPU y en el ejemplo, cuando el proceso 0 completa su I/O, se ejecuta inmediatamente después de haber estado bloqueado, aprovechando el 
     tiempo de la CPU de manera más eficiente.
   ## ¿Por qué podría ser recomendable ejecutar de nuevo un proceso que acaba de completar una I/O?

   Ejecutar de inmediato un proceso que acaba de completar una I/O puede ser ventajoso por varias razones:
   - Ejecutar el proceso de inmediato asegura que la CPU no se quede inactiva mientras espera que otros procesos estén listos, esto maximiza la utilización de los recursos de la 
    CPU, evitando tiempos de inactividad innecesarios.
   -  Al ejecutar el proceso inmediatamente después de completar la I/O, se reduce el tiempo de espera y se acelera el ciclo de vida de cada proceso, esto es especialmente útil en       sistemas con múltiples procesos, donde el tiempo de espera entre I/O y CPU puede acumularse, alargando innecesariamente el tiempo total de ejecución.
   - Si hay múltiples procesos con operaciones de I/O, ejecutar el proceso inmediatamente después de completar la I/O asegura que cada proceso se retome de manera oportuna, lo que       ayuda a balancear la carga del sistema y evitar bloqueos prolongados o inactividad de la CPU.
   </details>
   <br>


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
