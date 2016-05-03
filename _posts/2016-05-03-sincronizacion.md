---
layout: post
section-type: post
title: Algoritmo de sincronización
category: programacion
tags: [ 'php', 'sincronizar', 'csv', 'bd', 'base de datos', 'algoritmo']
---

Era una calurosa tarde de primavera en Granada, yo estaba tranquilo, mirando la pantalla del Mac que tengo en la oficina, cuando de 
repente escucho un David en la lejanía, me quito los cascos y espero una segunda llamada esperando que no me haya vuelto loco,
"David", ahí estaba, me levanté y fuí a ver que *pollas* querían ahora de mí. Quizás no fue tan poético como lo acabo de escribir,
pero la cosa es que, de repente, estaba allí de pie escuchando como me encargaban hacer un 'sincronizador' de clientes que 
estaban en un .csv y una tabla de clientes. Por si no lo sabes un archivo csv es una representación de una tabla, lo que podría ser

Paco; Lopez; 65789093; Murcia

Eso representaría una fila, por ejemplo. Pero en fin, lo que te estaba contando, me lo encargaron y lo primero que pensé fue: **"Vaya,
esto no puede ser tan difícil"** (jaja), una vez de vuelta al escritorio me puse a pensar, jamás había tenido que pensar en como hacer
un algoritmo que permitiera la sincronización y fuera algo rápido, así que me puse manos a la obra. Lo primero que me vino a la cabeza
fue coger la primera fila del archivo csv y buscar en la tabla la correspondencia, y si no estaba se creaba. Muy listo David, seguro 
que mirar uno a uno 9000 clientes para compararlos con otros 9000 es rápido, la cosa es que tendría que hacer 81 putos millones de
iteraciones, rapidísimo, vaya. Lo segundo que se me ocurrió fue ordenar ambas tablas y comparar linea a linea, claro, los que no tuvieran
correspondencia los guardaría en dos array, uno para los pobres clientes del csv que no fueron encontrados en la base de datos y otro
para el caso contrario, no hice cálculos, simplemente pensé que podría hacerlo mejor, así que seguí dándole vueltas y dibujando,
por fin hallé la solución, como podía ser tan tonto, los clientes tenían un ID **autoincremental**, y eso me ponía las cosas realmente
fáciles. Basicamente es esto:

```php
// Siendo el array donde se guardaban los .csv $csv, y el de la bd $clientes

$cont_csv = 0;
$cont_clientes = 0;

// Declaro un par de contadores

while($cont_csv < count($csv) || $cont_clientes < count($clientes)){
  if($cont_csv < count($csv) && $cont_clientes < count($clientes)){
    if($csv[$cont_csv]['ID'] < $clientes[$cont_clientes]['ID']){
      // Esto quiere decir que el que cogemos del csv no esta en la bd, así que lo insertamos;
      insert($csv[$cont_csv]);
      $cont_csv++;
    }
    elseif($csv[$cont_csv]['ID'] > $clientes[$cont_clientes]['ID']){
      // Esto quere decir que el de la bd no está en el csv, así que eliminamos el de la bd
      delete($clientes[$cont_clientes]);
      $cont_clientes++;
    }
    else{
      // Solo nos queda la opción de que sean iguales, así que updateamos a la versión del csv
      update($csv[$cont_csv]);
      $cont_clientes++;
      $cont_csv++;
    }
  }
  else{
    // Nos queda el caso en el que alguno de los dos haya acabado
    if($cont_clientes < count($clientes) && $cont_csv == count($csv)){
      // Significa que todos los que quedan en la bd no están en el csv, así que habría que eliminarlos
      while($cont_clientes < count($clientes)){
        delete($clientes[$cont_clientes]);
        $cont_clientes++;
      }
    }
    elseif($cont_clientes == count($clientes) && $cont_csv < count($csv)){
      // Este caso sería al revés, solo nos quedan en el csv, así que los insertamos
      while($cont_csv < count($csv)){
        insert($csv[$cont_csv]);
        $cont_csv++;
      }
    }
  }
}
```

Esta fua la lógica que alcancé, seguro que te parece fácil, seguro que se puede hacer mejor y seguro también 
que hay 3000 ya hechas por internet, pero realmente me interesaba hacerlo y por eso lo pensé y lo hice por mi cuenta.
