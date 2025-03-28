# Actividad de seguimiento - Simulación 1

| Integrante                | correo                     | usuario github |
| ------------------------- | -------------------------- | -------------- | --- |
| Sofia Vanegas Cordoba     | sofia.vanegasc@udea.edu.co | Sofito-code    |
| Samuel David Montoya Cano | samuel.montoya@udea.edu.co | SamuelMon      | .   |

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:

- Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
- Como la entrega se debe hacer modificando el archivo READNE, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
- Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.

   <details>
   <summary>Answer</summary>
   Viendo los parametros que se le pasan se puede saber que el uso de la cpu será de 100%, asi que primero se ejecutará un proceso y luego el otro.

   ![process-run con -l 5:100,5:100](./img/Imagen%201.png)

   Luego con las flags -c y -p comprobamos que el uso de la Cpu es del 100%.

   ![process-run con -l 5:100,5:100 -c -p](./img/imagen%202.png)
   </details>
   <br>

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right.

   <details>
   <summary>Answer</summary>
   Ya que el primer proceso tiene ocupada la CPU durante 4 unidades de tiempo y el segundo proceso que ejecuta instrucciones I/O sabemos que se demora 2 unidades de tiempo (cuando inicia y cuando termina) pero no sabemos cuanto tiempo se bloquea el proceso. Se demora más de 6 unidades de tiempo en terminar los 2 procesos.

   ![process-run con -l 4:100,1:0](./img/imagen%203.png)

   Luego con las flags -c y -p comprobamos que el proceso 2 se bloquea durante 5 unidades de tiempo por lo que el tiempo total es de 11 unidades de tiempo.

   ![process-run con -l 4:100,1:0 -c -p](./img/imagen%204.png)
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)

   <details>
   <summary>Answer</summary>
   Ahora va pasar que el primer proceso que corresponde a la ejecucion de intrucciones I/O va a bloquearse durante 5 unidades de tiempo y el proceso 2 va ejecutar las 4 intrucciones de CPU mientras el procecso 1 sigue bloqueado. El orden si importa porque mientras el proceso 1 se encuentra bloqueado, el proceso 2 puede ejecutarse.

   ![process-run con -l 1:0,4:100](./img/imagen%205.png)

   Luego con las flags -c y -p comprobamos que mientras el proceso 1 se encuentra bloqueado, el proceso 2 puede ejecutarse.

   ![process-run con -l 1:0,4:100 -c -p](./img/imagen%206.png)
   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?

   <details>
   <summary>Answer</summary>
   Con esta nueva flag el orden no importará y el tiempo total será de 11 unidades de tiempo ya que no se podrá aprovechar el tiempo de bloqueo del primer proceso.

   ![process-run con -l 1:0,4:100 -c -p -S SWITCH_ON_END](./img/imagen%207.png)

   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.

   <details>
   <summary>Answer</summary>
   Por el comportamiento observado anteriormente deberia ejecutarse el proceso 2 desde el instante 2, cuando el proceso 1 se bloquea.

   ![process-run con -l 1:0,4:100 -c -S SWITCH_ON_IO](./img/imagen%208.png)

   Observamos que correctamente se comporta como creiamos y se demora 7 unidades de tiempo al igual que antes.
   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?

   <details>
   <summary>Answer</summary>
   Creemos Que luego de lanzado el primer I/O del proceso 1 se ejecutaran los otros 3 procesos de CPU y luego seguira de nuevo el proceso 1. Al no comprender exactamente el efecto de la flag IO RUN LATER no estamos completamente seguros.

   ![process-run con -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER](./img/imagen%209.png)

   Luego de ejecutar con esta combinacion de flags confirmamos nuestras sospechas y el proceso con I/O fue el ultimo en terminar. Observamos que de esta forma no se aprovecha el tiempo que el proceso se bloquea por I/O y el tiempo de ejecucion total es mayor.
   </details>
   <br>

7. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?

   <details>
   <summary>Answer</summary>
   Basandonos en el punto 6 conde comentamos que no se aprovecha el tiempo de en el que el proceso está bloqueado por I/O,
   creemos que con esta flag este tiempo si se aprovechará y el tiempo total de ejecucion será menor.

   ![process-run con -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN IMMEDIATE](./img/imagen%2010.png)

   Efectivamente de esta forma se aprovecha el tiempo en el que el proceso bloqueado por I/O para ejecutar los demas procesos y el tiempo total de ejecucion disminuyó en 10 unidades.
   </details>
   <br>

### Criterios de evaluación

- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
