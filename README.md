# Simulador Visual de Pila Recursiva RISC-V  
  
![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-green.svg)  
![Estado](https://img.shields.io/badge/Estado-Estable-brightgreen)  
![HTML5](https://img.shields.io/badge/HTML5-Web-orange)  
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow)  
![Tailwind CSS](https://img.shields.io/badge/TailwindCSS-CDN-38B2AC)  
![Arquitectura](https://img.shields.io/badge/Arquitectura-RISC--V-red)  
![Tipo](https://img.shields.io/badge/Proyecto-Educativo-blue)  
![UCM](https://img.shields.io/badge/Universidad-Complutense%20de%20Madrid-7A003C)  
  
Simulador educativo interactivo para visualizar, paso a paso, la ejecución de código ensamblador **RISC-V** con foco en la **recursividad**, la **gestión de la pila (stack)** y el **estado de los registros del procesador**.  
  
---  
  
## Autor  
  
**Pascal**    
**Universidad Complutense de Madrid (UCM)**    
**Fundamentos de Computadores II — Práctica 3 (Modificación)**  
  
---  
  
## Descripción del proyecto  
  
Esta aplicación web es un simulador educativo **sin dependencias de ejecución** diseñado para mostrar de forma visual cómo se comporta la memoria  y el banco de registros durante la ejecución de una función estrictamente recursiva en **RISC-V**.  
  
El algoritmo simulado es una implementación recursiva del **producto escalar** (`dotprod`) entre dos vectores enteros. La simulación sigue de manera rigurosa los convenios oficiales de llamada a subrutinas de RISC-V:  
  
- creación del marco de pila en el prólogo,  
- preservación de registros salvados,  
- gestión del `ra` (*return address*),  
- restauración del contexto en el epílogo.  
  
El objetivo principal es que el estudiante pueda **ver** lo que normalmente solo aparece de forma abstracta en teoría o en ensamblador plano.  
  
---  
  
## Objetivos de aprendizaje  
  
Este simulador ha sido creado para ayudar a comprender visualmente conceptos clave de arquitectura de computadores:  
  
- Entender por qué la pila crece hacia **direcciones descendentes**.  
- Visualizar múltiples **stack fes** coexistiendo en memoria durante la recursividad.  
- Comprender por qué es necesario preservar `ra` y los registros salvados (`s1-s11`, `fp`) antes de ejecutar una llamada.  
- Observar cómo el procesador restaura el estado previo de ejecución al desapilar durante el retorno recursivo.  
- Relacionar instrucciones ensamblador concretas con su efecto real sobre memoria y registros.  
  
---  
  
## Base matemática y algorítmica  
  
El simulador resuelve el producto escalar recursivo mediante la relación de recurrencia:  
  

Resultado = mul(V[n-1], W[n-1]) + dotprod(V, W, n-1)

### Significado de cada término

-   **`Resultado`**: valor escalar final devuelto por la función, almacenado en el registro de retorno principal `a0`.
    
-   **`mul`**: subrutina auxiliar encargada de multiplicar dos enteros sin usar extensión de multiplicación por hardware.
    
-   **`V[n-1]`**: valor de la última posición válida actual del primer vector.
    
-   **`W[n-1]`**: valor de la última posición válida actual del segundo vector.
    
-   **`dotprod`**: llamada recursiva a la propia función principal.
    
-   **`V, W`**: direcciones base en memoria de los arreglos originales.
    
-   **`n-1`**: tamaño reducido del problema en cada nivel de recursión.
    

Cuando `n = 0`, se alcanza el **caso base** y finaliza la recursividad.

---

## Cómo funciona la simulación

La herienta se divide en **tres paneles sincronizados en tiempo real**:

### 1. Panel de código ensamblador

Muestra el código fuente ejecutado línea a línea.  
La instrucción actual se resalta para facilitar el seguimiento del flujo del proga, incluidos:

-   saltos condicionales,
    
-   llamadas a subrutinas,
    
-   retornos.
    

### 2. Panel de registros y explicación

Presenta una explicación funcional y matemática en lenguaje natural de la instrucción actual. Por ejemplo:

-   `lw`
    
-   `addi`
    
-   `bge`
    
-   `call`
    

Además, muestra el estado en vivo de:

-   registros de argumento (`a0-a7`),
    
-   registros salvados (`fp`, `s1-s11`),
    
-   registros temporales (`t0-t6`).
    

Los registros modificados en el ciclo actual se iluminan para mejorar la legibilidad.

### 3. Panel de memoria  (pila)

Representa visualmente la memoria mediante coordenadas absolutas, diferenciando entre:

-   espacio reservado pero aún vacío,
    
-   datos válidos almacenados con etiquetas como `[ra]` o `[fp]`,
    
-   memoria obsoleta tras el movimiento del puntero de pila `sp`.
    

Esto permite observar físicamente el crecimiento y decrecimiento de la pila durante la ejecución.

---

## Características principales

-   Simulación paso a paso de ejecución en RISC-V.
    
-   Visualización de la pila de llamadas en tiempo real.
    
-   Seguimiento del estado de registros del procesador.
    
-   Explicaciones pedagógicas de cada instrucción.
    
-   Enfoque específico en la recursividad y los marcos de activación.
    
-   Aplicación web estática y autocontenida.
    
-   Sin instalación de dependencias ni servidor local.
    

---

## Limitaciones técnicas

Este software ha sido diseñado como una herienta **educativa** y, por tanto, incorpora ciertas simplificaciones deliberadas:

### Direcciones de memoria

El simulador fija el techo de la pila en la dirección:

```
Plain text

0x20000
```

En entornos reales o en otros simuladores como **RARS**, **Venus** o hardware físico, la pila puede inicializarse en direcciones distintas.

### Uso de registros temporales

La asignación de registros temporales (`t0`, `t1`, `t2`, etc.) es puente ilustrativa. Una implementación real puede variar según:

-   el progador,
    
-   el compilador,
    
-   el nivel de optimización.
    

### Abstracción de subrutinas

La subrutina auxiliar `mul` se resuelve de forma analítica en un solo paso. Esta decisión es intencional para no desviar la atención del foco principal del simulador: la gestión de la pila recursiva de `dotprod`.

---

## Instalación y uso

La aplicación es un entorno web **estático** y **autocontenido**.

No requiere:

-   instalación de software adicional,
    
-   servidor local,
    
-   paquetes externos de ejecución.
    

> El estilo visual de Tailwind CSS se carga vía CDN.

### Pasos para ejecutar

1.  Descarga el archivo `simulador_pila.html`.
    
2.  Ábrelo con doble clic en cualquier navegador moderno:
    
    -   Google Chrome
        
    -   Mozilla Firefox
        
    -   Microsoft Edge
        
    -   Safari
        
3.  Usa el botón **Siguiente Paso** para avanzar instrucción a instrucción.
    
4.  Utiliza la barra de progreso inferior para desplazarte rápidamente a estados más profundos de la recursividad.
    

---

## Casos de uso recomendados

Este simulador resulta especialmente útil para:

-   prácticas de arquitectura de computadores,
    
-   clases sobre recursividad a bajo nivel,
    
-   visualización de llamadas a subrutinas,
    
-   estudio del convenio de llamadas en RISC-V,
    
-   apoyo docente en laboratorio o tutorías.
    

---

## Tecnologías utilizadas

-   **HTML5**
    
-   **JavaScript Vanilla**
    
-   **Tailwind CSS (CDN)**
    
-   **RISC-V Assembly** como modelo conceptual de simulación
    

---

## Licencia

Este proyecto está distribuido bajo licencia **MIT**.

### MIT License

Copyright (c) 2026 **Pascal**  
**Universidad Complutense de Madrid (UCM)**

Se concede permiso, de forma gratuita, a cualquier persona que obtenga una copia de este software y de los archivos de documentación asociados (el "Software"), para utilizar el Software sin restricción, incluyendo sin limitación los derechos de usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y/o vender copias del Software, y permitir a las personas a las que se les proporcione el Software hacer lo mismo, sujeto a las siguientes condiciones:

El aviso de copyright anterior y este aviso de permiso deberán incluirse en todas las copias o partes sustanciales del Software.

EL SOFTWARE SE PROPORCIONA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O IMPLÍCITA, INCLUYENDO PERO NO LIMITADO A GARANTÍAS DE COMERCIALIZACIÓN, IDONEIDAD PARA UN PROPÓSITO PARTICULAR Y NO INFRACCIÓN. EN NINGÚN CASO LOS AUTORES O TITULARES DEL COPYRIGHT SERÁN RESPONSABLES DE NINGUNA RECLAMACIÓN, DAÑOS U OTRAS RESPONSABILIDADES, YA SEA EN UNA ACCIÓN DE CONTRATO, AGRAVIO O CUALQUIER OTRO MOTIVO, QUE SURJA DE O EN CONEXIÓN CON EL SOFTWARE O EL USO U OTROS TRATOS EN EL SOFTWARE.

---
