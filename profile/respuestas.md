# Profiling:

En primer lugar, se cambia el tamaño de las matrices en profile\_me\_1.c
(de 5000 a 500) para que no tire un segmentation fault. Luego, se crea
un Makefile para compilar los dos códigos. En ese Makefile, se compila
con los flags ´-O\_´, con los niveles de optimización 0, 1 y 3.

### Profile\_me\_1

#### Con `time`

Las corridas con las distintas optimizaciones dan:

```
$time ./profile_me_1_O0.e 

real    0m0.056s
user    0m0.044s
sys     0m0.008s
```

```
$time ./profile_me_1_O1.e 

real    0m0.054s
user    0m0.048s
sys     0m0.008s
```

```
$time ./profile_me_1_O3.e 

real    0m0.003s
user    0m0.000s
sys     0m0.000s
```


#### Con `gprof`

En este caso, hay que correr con el flag ´-pg´, tanto en la
compilación como en el linkeo. Luego del correr el programa, se genera un
archivo, gmon.out, que se utiliza para ver los tiempos del
programa. Para cada uno de los casos:

´´´
$ gprof profile\_me\_1\_O0.e gmon.out > profile\_me\_1\_O0.prof


Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total
   time   seconds   seconds    calls  ns/call  ns/call  name
    50.32      0.02     0.02   250000    80.52    80.52  second_assign
    25.16      0.03     0.01   250000    40.26    40.26  first_assign
    25.16      0.04     0.01                             main
´´´   

´´´
$ gprof profile\_me\_1\_O1.e gmon.out > profile\_me\_1\_O1.prof


Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total
   time   seconds   seconds    calls  ns/call  ns/call  name
    50.32      0.02     0.02                             _fini
    25.16      0.03     0.01   250000    40.26    40.26  __libc_csu_init
    25.16      0.04     0.01   250000    40.26    40.26  main
´´´

´´´
$ gprof profile\_me\_1\_O3.e gmon.out > profile\_me\_1\_O3.prof


Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total
   time   seconds   seconds    calls  Ts/call  Ts/call  name
   100.65      0.04     0.04                             \_fini
´´´

Se observa que en el primer caso pueden diferenciarse bien las
funciones, pero una vez que se ponen flags de optimización, ya no.




#### Con `perf`

Para cada uno de los casos:

´´´
$ perf stat ./profile\_me\_1\_O0.e 

 Performance counter stats for './profile\_me\_1\_O0.e':

         54.975644      task-clock (msec)         #    0.947 CPUs utilized          
                12      context-switches          #    0.218 K/sec                  
                 0      cpu-migrations            #    0.000 K/sec                  
             1,519      page-faults               #    0.028 M/sec                  
       102,673,500      cycles                    #    1.868 GHz                      (52.67%)
         4,252,636      stalled-cycles-frontend   #    4.14% frontend cycles idle     (56.26%)
        77,709,647      stalled-cycles-backend    #   75.69% backend  cycles idle     (57.39%)
        65,421,789      instructions              #    0.64  insns per cycle        
                                                  #    1.19  stalled cycles per insn  (55.03%)
         6,979,318      branches                  #  126.953 M/sec                    (47.89%)
           477,402      branch-misses             #    6.84% of all branches          (42.69%)

       0.058067696 seconds time elapsed
´´´

´´´
$ perf stat ./profile\_me\_1\_O1.e 

 Performance counter stats for './profile\_me\_1\_O1.e':

         56.977205      task-clock (msec)         #    0.972 CPUs utilized          
                15      context-switches          #    0.263 K/sec                  
                 0      cpu-migrations            #    0.000 K/sec                  
             1,515      page-faults               #    0.027 M/sec                  
       111,388,964      cycles                    #    1.955 GHz                      (54.10%)
        14,794,109      stalled-cycles-frontend   #   13.28% frontend cycles idle     (57.27%)
        79,034,009      stalled-cycles-backend    #   70.95% backend  cycles idle     (58.41%)
        29,094,096      instructions              #    0.26  insns per cycle        
                                                  #    2.72  stalled cycles per insn  (51.66%)
         4,637,170      branches                  #   81.386 M/sec                    (44.80%)
         1,084,752      branch-misses             #   23.39% of all branches          (50.48%)

       0.058613431 seconds time elapsed
´´´

´´´
$ perf stat ./profile\_me\_1\_O3.e 

 Performance counter stats for './profile\_me\_1\_O3.e':

          0.728400      task-clock (msec)         #    0.196 CPUs utilized          
                 1      context-switches          #    0.001 M/sec                  
                 0      cpu-migrations            #    0.000 K/sec                  
                50      page-faults               #    0.069 M/sec                  
         1,346,205      cycles                    #    1.848 GHz                    
           457,584      stalled-cycles-frontend   #   33.99% frontend cycles idle   
           778,438      stalled-cycles-backend    #   57.82% backend  cycles idle   
           569,352      instructions              #    0.42  insns per cycle        
                                                  #    1.37  stalled cycles per insn  (12.45%)
     <not counted>      branches                   (0.00%)
     <not counted>      branch-misses              (0.00%)

       0.003723938 seconds time elapsed
´´´

