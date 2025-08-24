---
title: Expresiones regulares
---
Espacio dedicado a la recopilación de expresiones regulares organizadas por categorías como validación de correos electrónicos, búsqueda de patrones y más. Cada expresión incluye una breve descripción y está diseñada para facilitar el acceso rápido a la información, optimizando así tu flujo de trabajo en el desarrollo.

### `\d+`

- **Descripción:** Coincide con uno o más dígitos numéricos.
    
- **Ejemplo:** Coincide con _"123"_, _"4567"_.
    
- **Desglose:**
    
    - `\d`: Coincide con cualquier dígito (0-9).
        
    - `+`: Indica que el dígito anterior (`\d`) puede aparecer una o más veces.
        

---

### `\w{3,5}`

- **Descripción:** Coincide con una palabra que tiene entre 3 y 5 caracteres alfanuméricos.
    
- **Ejemplo:** Coincide con _"abc"_, _"xyz12"_, pero no con _"ab"_ ni con _"abcdefg"_.
    
- **Desglose:**
    
    - `\w`: Coincide con cualquier carácter alfanumérico (letras y números).
        
    - `{3,5}`: Indica que debe haber entre 3 y 5 caracteres alfanuméricos.
        

---

### `[a-zA-Z]`

- **Descripción:** Coincide con cualquier letra mayúscula o minúscula.
    
- **Ejemplo:** Coincide con _"A"_, _"b"_, _"Z"_, pero no con _"1"_ ni con _"%"_.
    
- **Desglose:**
    
    - `[a-zA-Z]`: Coincide con cualquier letra mayúscula o minúscula.
        

---

### `\b\d{2,4}\b`

- **Descripción:** Coincide con un número de 2 a 4 dígitos como una palabra completa.
    
- **Ejemplo:** Coincide con _"123"_, _"4567"_, pero no con _"12"_ ni con _"12345"_.
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `\d{2,4}`: Coincide con un número de 2 a 4 dígitos.
        

---

### `^Hola`

- **Descripción:** Coincide con _"Hola"_ al principio de una cadena.
    
- **Ejemplo:** Coincide con _"Hola mundo"_, pero no con _"¡Hola!"_ ni con _"Mundo, Hola"_.
    
- **Desglose:**
    
    - `^`: Coincide con el principio de una cadena.
        
    - `Hola`: Coincide literalmente con la palabra _"Hola"_.
        

---

### `\d{3}-\d{2}-\d{4}`

- **Descripción:** Coincide con un formato de número de seguro social en el estilo _123-45-6789_.
    
- **Ejemplo:** Coincide con números en el formato _"123-45-6789"_.
    
- **Desglose:**
    
    - `\d{3}`: Coincide con tres dígitos.
        
    - `-`: Coincide con un guion.
        
    - `\d{2}`: Coincide con dos dígitos.
        
    - `-`: Coincide con otro guion.
        
    - `\d{4}`: Coincide con cuatro dígitos.
        

---

### `[^aeiou]`

- **Descripción:** Coincide con cualquier carácter que no sea una vocal.
    
- **Ejemplo:** Coincide con _"b"_, _"1"_, _"%"_ pero no con _"a"_, _"e"_ o _"o"_.
    
- **Desglose:**
    
    - `[^aeiou]`: Coincide con cualquier carácter que no sea una vocal (a, e, i, o, u).
        

---

### `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b`

- **Descripción:** Coincide con una dirección de correo electrónico válida.
    
- **Ejemplo:** Coincide con direcciones de correo electrónico como _"user@example.com"_.
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `[A-Za-z0-9._%+-]+`: Coincide con uno o más caracteres alfanuméricos, punto, guion bajo, porcentaje o signo más o menos.
        
    - `@`: Coincide con el símbolo "@".
        
    - `[A-Za-z0-9.-]+`: Coincide con uno o más caracteres alfanuméricos, punto o guion.
        
    - `\.`: Coincide con un punto (debe ser escapado con `\`).
        
    - `[A-Z|a-z]{2,}`: Coincide con dos o más letras mayúsculas o minúsculas.
        
    - `\b`: Marca el límite de una palabra.
        

---

### `\b(\w+)\s\1\b`

- **Descripción:** Coincide con palabras duplicadas, como _"hola hola"_ o _"123 123"_.
    
- **Ejemplo:** Coincide con palabras duplicadas como _"hola hola"_ o _"123 123"_.
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `(\w+)`: Captura una palabra (cualquier carácter alfanumérico) y la almacena en un grupo.
        
    - `\s`: Coincide con un espacio en blanco.
        
    - `\1`: Hace referencia al contenido del primer grupo capturado.
        
    - `\b`: Marca el límite de una palabra.
        

---

### `\b(?i)regex\b`

- **Descripción:** Coincide con la palabra _"regex"_ de manera insensible a mayúsculas y minúsculas.
    
- **Ejemplo:** Coincide con _"regex"_, _"RegEx"_, _"REGEX"_, etc.
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `(?i)`: Activa la coincidencia insensible a mayúsculas y minúsculas.
        
    - `regex`: Coincide literalmente con la palabra _"regex"_.
        
    - `\b`: Marca el límite de una palabra.
        

---

### `\d{1,3}(,\d{3})*(\.\d+)?`

- **Descripción:** Coincide con un número decimal con o sin comas de mil, por ejemplo, _"1,000"_ o _"123.45"_.
    
- **Ejemplo:** Coincide con números como _"123"_, _"1,234"_, _"567.89"_.
    
- **Desglose:**
    
    - `\d{1,3}`: Coincide con uno a tres dígitos.
        
    - `(,\d{3})*`: Coincide con cero o más grupos de una coma seguida por exactamente tres dígitos.
        
    - `(\.\d+)?`: Coincide con un punto seguido por uno o más dígitos, opcionalmente.
        

---

### `\b(?<!@)\w+\b`

- **Descripción:** Coincide con palabras que no están precedidas por el símbolo "@".
    
- **Ejemplo:** Coincide con palabras que no están precedidas por "@".
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `(?<!@)`: Asegura que la palabra no está precedida por el símbolo "@".
        
    - `\w+`: Coincide con una o más letras, dígitos o guiones bajos.
        
    - `\b`: Marca el límite de una palabra.
        

---

### `\b(?:\d{1,3}\.){3}\d{1,3}\b`

- **Descripción:** Coincide con direcciones IP como _"192.168.1.1"_.
    
- **Ejemplo:** Coincide con direcciones IP como _"192.168.1.1"_.
    
- **Desglose:**
    
    - `\b`: Marca el límite de una palabra.
        
    - `(?: ... )`: Agrupa sin capturar.
        
    - `\d{1,3}\.`: Coincide con uno a tres dígitos seguidos por un punto.
        
    - `{3}`: Indica que la secuencia anterior debe repetirse tres veces.
        
    - `\d{1,3}`: Coincide con uno a tres dígitos.
        
    - `\b`: Marca el límite de una palabra.
        

---

### `https?://\S+`

- **Descripción:** Coincide con URLs que comienzan con _"http://"_ o _"https://"_.
    
- **Ejemplo:** Coincide con URLs como _"[http://example.com](http://example.com)"_ o _"[https://www.example.com](https://www.example.com)"_.
    
- **Desglose:**
    
    - `https?`: Coincide con "http" seguido de una "s" opcional.
        
    - `://`: Coincide con "://".
        
    - `\S+`: Coincide con uno o más caracteres que no son espacios en blanco.
        

---

### `(0[1-9]|1[0-2])/(0[1-9]|[12][0-9]|3[01])/\d{4}`

- **Descripción:** Coincide con fechas en formato _"MM/DD/YYYY"_.
    
- **Ejemplo:** Coincide con fechas en formato _"MM/DD/YYYY"_.
    
- **Desglose:**
    
    - `(0[1-9]|1[0-2])`: Coincide con un número de 01 a 12 (mes).
        
    - `/`: Coincide con "/".
        
    - `(0[1-9]|[12][0-9]|3[01])`: Coincide con un día válido, de 01 a 31.
        
    - `/`: Coincide con "/".
        
    - `\d{4}`: Coincide con un año de cuatro dígitos.
        

---

### `^(0[1-9]|1[0-2]):[0-5][0-9] (AM|PM)$`

- **Descripción:** Coincide con horas en formato _"HH:MM AM/PM"_.
    
- **Ejemplo:** Coincide con horas en formato _"HH:MM AM/PM"_.
    
- **Desglose:**
    
    - `^`: Coincide con el principio de la cadena.
        
    - `(0[1-9]|1[0-2])`: Coincide con una hora de 01 a 12.
        
    - `:`: Coincide con ":".
        
    - `[0-5][0-9]`: Coincide con minutos de 00 a 59.
        
    - `(AM|PM)`: Coincide con un espacio seguido por _"AM"_ o _"PM"_.
        
    - `$`: Coincide con el final de la cadena.
        

---

### `^#(?:[0-9a-fA-F]{3}){1,2}$`

- **Descripción:** Coincide con códigos de color hexadecimal, como _"#1a3"_ o _"#aabbcc"_.
    
- **Ejemplo:** Coincide con códigos de color hexadecimal como _"#1a3"_ o _"#aabbcc"_.
    
- **Desglose:**
    
    - `^`: Coincide con el principio de la cadena.
        
    - `#`: Coincide con el símbolo de almohadilla.
        
    - `(?:[0-9a-fA-F]{3}){1,2}`: Coincide con uno o dos conjuntos de tres caracteres hexadecimales.
        
    - `$`: Coincide con el final de la cadena.
        

---

### `^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$`

- **Descripción:** Exige al menos 8 caracteres, una mayúscula, una minúscula, un número y un carácter especial.
    
- **Ejemplo:** Valida contraseñas que cumplen con los requisitos.
    
- **Desglose:**
    
    - `^`: Coincide con el principio de la cadena.
        
    - `(?=.*[a-z])`: Asegura que hay al menos una letra minúscula.
        
    - `(?=.*[A-Z])`: Asegura que hay al menos una letra mayúscula.
        
    - `(?=.*\d)`: Asegura que hay al menos un dígito.
        
    - `(?=.*[@$!%*?&])`: Asegura que hay al menos un carácter especial.
        
    - `[A-Za-z\d@$!%*?&]{8,}`: Coincide con al menos 8 caracteres que pueden ser letras (mayúsculas o minúsculas), dígitos o algunos caracteres especiales.
        
    - `$`: Coincide con el final de la cadena.