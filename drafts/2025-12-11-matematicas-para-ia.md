---
title: Matematicas para IA
image:
    path: /assets/posts/2025-12-11-matematicas-para-ia/portada.jpeg
date: 2025-12-11 04:04:45 +/-TTTT
categories: [AIRedTeamer]
tags: [AI Red Teamer, Machine Learning, Artificial Intelligence, Deep Learning]
---

## Operaciones aritmeticas basicas

### Multiplicacion

```
2 * 2 = 4
```

### Division

```
20 / 5 = 13
```

### Resta

```
5 - 3 = 2
```

### Suma

```
2 + 2 = 4
```

## Notaciones Algebraicas

### Notacion de Subindice

La notación subíndice representa una variable indexada por t, que a menudo indica un paso de tiempo específico o un estado en una secuencia. Por ejemplo:
```
x_t = q( x_t | x_{t-2} )
```
Esta notación se utiliza habitualmente en secuencias y series temporales, donde cada x_t representa el valor de x en el momento t.

x_t se lee “x en el tiempo de t”, el subíndice t es solo un número que marca el turno: día 1, día 2, día 3… (o paso 1, paso 2, paso 3).

**Whats?**

Imagina que llevás un cuaderno donde anotas algo **cada día**.

x_1 : lo que paso el dia 1
x_2: lo que paso el dia 2
x_3: lo que paso el dia 3
…y asi.

#### Qué significa **x_t = q ( x_t \ x_{t−2} )** ?

- **x_t** : “lo que pasa hoy (en el tiempo t)”
- **x_t - 2**: “lo que paso hace dos dias”
- La barrita \ se lee “dado” o “segun”
- **q** : Es una regla o modelo que usa lo de hace dos dias para decir como es hoy.    
Entonces, se lee así:
> “Hoy lo decido con una regla que mira lo de hace 2 días.”


### Notacion Superindice

La notación en superíndice se utiliza para denotar exponentes o potencias. Por ejemplo:
```
x^2 = x * x
```

Si X=2
X^2 = 2 * 2 = 4
X^3= 2 * 2 * 2 = 8

### Norma

La norma mide el tamaño o la longitud de un vector. La norma más común es la norma euclidiana, que se calcula de la siguiente manera:
```
||v|| = sqrt{v_1^2 + v_2^2 + ... + v_n^2}
```
Otras normas incluyen la norma L1 (distancia de Manhattan) y la norma L∞ (valor absoluto máximo):
```
||v||_1 = |v_1| + |v_2| + ... + |v_n|
||v||_∞ = max(|v_1|, |v_2|, ..., |v_n|)
```
Las normas se utilizan en diversas aplicaciones, como medir la distancia entre vectores, regularizar modelos para evitar el sobreajuste y normalizar datos.

**Que es un  vector?**  
Un vector es solo **una lista corta de números**. En 2D, por ejemplo, v=(x,y).
Podés imaginarlo como **una flecha** desde (0,0) hasta el punto (x,y).

**Idea basica para pringaos de lo que es una Norma**  
La **norma** es “**qué tan larga es la flecha**”.  
Distintas normas = **distintas reglas** para medir esa longitud.  


**Tres reglas famosas (con idea visual)**  
1 - L2 (euclidiana) = diagonal directa   
![alt text](/assets/posts/2025-12-11-matematicas-para-ia/1.png)  
Es la cinta métrica “normal” en línea recta.

2 - L1 (Manhattan) = por las cuadras    
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/2.png)  
Solo podés ir derecha/izquierda y arriba/abajo (como calles en cuadrícula).


3 - L∞ (infinito) = el desvío más grande  
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/3.png)  
Mira qué tanto te alejaste en una dirección (la mayor de las dos).

**Frase que te lo fija**
> Norma = tamaño de la flecha. L2 mide en diagonal, L1 por cuadras, L∞ se queda con el desvío más grande.

### Simbolo de Suma
Indica la suma de una secuencia de términos. Por ejemplo:  
```
Σ_{i=1}^{n} a_i
```
Esto representa la suma de los términos a_1, a_2, ..., a_n. La suma se utiliza en muchas fórmulas matemáticas, incluyendo el cálculo de medias, varianzas y series.

Esto representa la suma de los términos a_1, a_2, ..., a_n. La suma se utiliza en muchas fórmulas matemáticas, incluyendo el cálculo de medias, varianzas y series.

Ejemplo general:  
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/4.png)  
Se lee: “**suma los a_i desde i=1  hasta i=n”**.  
Es decir: a1+a2+a3+⋯+an

## Logaritmos y exponenciales



<!-- ![alt text](./assets/posts/2025-12-11-matematicas-para-ia/5.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/6.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/7.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/8.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/9.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/10.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/11.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/12.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/13.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/14.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/15.png) 
![alt text](./assets/posts/2025-12-11-matematicas-para-ia/16.png)  -->