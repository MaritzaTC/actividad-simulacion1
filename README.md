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
     -  Process 0:omenzó a ejecutarse y ocupó la CPU durante 4 unidades de tiempo, como esperábamos.
     - Process 1: Emitió una operación de I/O en el tiempo 5. Este proceso se bloqueó después de emitir la I/O, ya que no se puede ejecutar hasta que la operación de I/O       
     termine. 
       El proceso permaneció bloqueado [6-10] hasta que la I/O se completó.
       Finalmente, en el tiempo 11, Proceso 1 completó la operación de I/O y ejecutó la instrucción io_done, terminando así su ejecución.
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)
   
   <details>
   <summary>Answer</summary>
   Al ejecutar el comando tenemos: 
   <br> 
   ![6](https://github.com/user-attachments/assets/c57d338c-8ae1-4902-ac19-1aa48812c20b)

   ## 
   </details>
   <br>

5. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

6. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

7. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

8. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
