# Expresiones regulares
**NOTA:** En la mayoría de los ejemplos de cada símbolo estaremos dando los mismo ejemplos pero con el respectivo funcionamiento.

## Símbolos
### Punto `.`
El punto `.` representa un carácter el cual puede se un espacio ` `, tabulador `	`, números `0-9`, letras `a-Z`, etc.

**NOTA:** Normalmente cualquier caracteres de del estándar ASCII o cualquier otra decodificación. Como pueden ver no tiene que ser caracteres visibles, como por ejemplo el espacio y el tabulador `	`.

**Ejemplos**
 * `.` #Cuando hacemos una búsqueda con regexp y usamos tan solo el punto, el seleccionar cada uno de los caracteres. Eso quiere decir que selecciona caracteres individuales y si queremos remplazar la siguiente cadena `Hola a todos!` con la expresión `.` por el carácter `*`. Este seria el resultado `Hola a todos!` **[string (13)]** => `*************` **[string (13)]** dónde cada uno se a remplazado por un asterisco.
 * `....` #Si hacemos el mismo proceso pero esta vez con 4 puntos como expresiones `....` el resultado serial el siguiente `Hola a todos!` **[string (13)]** => `***!` **[string (4)]**, como se puede ver la cadena se a reducido de 13 caracteres a 4. Eso se debe a que en cada comienzo de línea selecciona los primeros 4 caracteres `Hola` => `*` después sigue con ` a t` => `*` y por ultimó `odos` => `*` y como al final con el signo de exclamaciones no puede agrupar 4 caracteres no hace nada y si tuviéramos mas lines sigue el proceso en esa línea.
 * `.a` #siguiendo con el mismo ejemplo ahora lo que este expresión esta buscando es cualquier carácter y que delante de el haya una `a`. Así que el resultado con la misma cadena seria el siguiente `Hola a todos!` **[string (13)]** => `ho** todos!` **[string (4)]**.
## Clases
### Números `\d`
el `\d` esto es equivalente a `[0-9]` que hacen referencia a los números `0-9`.

**Ejemplos**
 * `\d` #Al igual que en el punto este selecciona cada uno de los dígitos por separado. veamos esto con la siguiente cadena `23 de mayo del 1987` **[string (19)]** => `** de mayo del ****` **[string (19)]**
 * `\d\d\d` #Con esta expresión se busca tres números consecutivos, vamos al ejemplo! `23 de mayo del 1987` **[string (19)]** => `23 de mayo del *7` **[string (17)]**
 * `\d.` #En este ejemplo hacemos la búsqueda de un número que venga seguido de cualquier carácter. Este seria el resultado `23 de mayo del 1987` **[string (19)]** => `* de mayo del **` **[string (16)]**

### Carácter de una palabra `\w`
el `\w` esto es equivalente a `[a-zA-Z0-9_]` que hace referencia a los números `0-9`, letras `a-Z` y el guion bajo `_` cualquier otro carácter sera ignorado.

**NOTA:** Acá no se tiene en cuenta las palabras que no estén en el estándar ASCII, como por ejemplo la `ñ` o letras con tildes `á-Ú`, etc.

**Ejemplos**
 * `\w` #Al igual que en el punto este selecciona cada uno de los caracteres que este representa por separado. veamos esto con la siguiente cadena `23 de mayo del 1987` **[string (19)]** => `** ** **** *** ****` **[string (19)]**. Como se puede ver este a ignorado los espacios.
 * `\w\w\w` #Con esta expresión se busca tres caracteres consecutivos que el representa, `23 de mayo del 1987` **[string (19)]** => `23 de *o * *7` **[string (13)]**
 * `\w.` #En este ejemplo hacemos la búsqueda de caracteres consecutivos que el representa que venga seguido de cualquier carácter. Este seria el resultado `23 de mayo del 1987` **[string (19)]** => `* * ** ****` **[string (11)]**, En este ejemplo el resultado es muy similar al resultado de `\w\w` con diferencia en la palabra `del ` donde `de` => `*` y `l ` => `*`.

**Ya que estamos entendiendo como funcionan las expresiones vamos a dar ejemplos mas enfocados o evitarlos**

### espacio `\s`
Como en todos los otros ejemplo este solo hace referencia a los espacios.

`*` que se encuentre 0 o mas veces y trae el match mas largo.
`+` que se encuentre 1 o mas veces y trae el match mas largo.
`?` que se encuentre 0 o una vez y trae el match mas largo.

`{min,max}` que se repita mínimo `min` y máximo `max` si no se llena el máximo este sera hasta el infinito.

`[...]` Clase donde se puede concatenar diferentes caracteres.

`*?` que se encuentre 0 o mas veces y trae el match mas corto.
`+?` que se encuentre 1 o mas veces y trae el match mas corto.
`??` que se encuentre 0 o una vez y trae el match mas corto.

`[^...]` excluir todo lo que esta en la case

`\D` a diferencia de `/d` excluye
`\w` a diferencia de `/W` excluye
`\s` a diferencia de `/S` excluye

`^...$` para señalar comienzo de línea `^` y fin de línea `$`

`(...)` Selecciona un regex y el resultado lo asigna a una variable, Esto dependiendo en el lenguaje de programación que lo implemente, en los editores de texto suelte ser `$0` match completo `$n` donde n equivale a un numero, guarda el grupo.
**Ejemplo**
`^(\d),(\d),\d,.*$` donde la primera columna del csv. se guarda en `$1` y la segunda columna ern `$2`, el resto del match no se guarda