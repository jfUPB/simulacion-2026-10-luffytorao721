# Unidad 1

## Bitácora de proceso de aprendizaje

## **Actividad 1**
### **La aleatoriedad en el arte generativo**

- *La posibilidad de crear cosas nuevas sin dejar cabida a la adaptabilidad, dejando como resultado una obra única.*

---
## **Actividad 2**

#### Antes de ejecutar el código modificado, esperaba que el punto siguiera moviéndose de manera aleatoria, pero que con el paso del tiempo se notara una tendencia clara hacia la derecha del lienzo. Aunque cada movimiento individual sería impredecible, 
consideré que al darle mayor probabilidad a una dirección específica el recorrido general dejaría de estar equilibrado alrededor del centro y se desplazaría progresivamente hacia ese lado.
Después de ejecutar el código, observé que el punto comenzó en el centro del canvas y se movió de forma irregular dejando un rastro continuo. El comportamiento seguía siendo aleatorio en cada paso, sin embargo, tras unos segundos fue evidente que 
el recorrido se extendía mucho más hacia la derecha que hacia las demás direcciones, confirmando que el patrón general no permanecía centrado.
Esto coincidió con lo que había anticipado, ya que al modificar las probabilidades de movimiento se introduce un sesgo que, aunque no elimina el azar, sí influye en el resultado a largo plazo. La caminata demuestra que un sistema puede parecer 
caótico en el corto plazo, pero aun así generar patrones reconocibles cuando se observan muchos pasos consecutivos.

---

## ** Actividad 3**



### *Diferencia entre distribución uniforme y no uniforme*

Una distribución uniforme de números aleatorios es aquella en la que todos los valores posibles tienen la misma probabilidad de ocurrir, por lo que no existe ninguna preferencia o tendencia hacia un valor en particular. En este caso, el comportamiento resultante suele verse equilibrado y simétrico, ya que con el tiempo los valores se reparten de manera homogénea. En cambio, una distribución no uniforme asigna distintas probabilidades a los valores, haciendo que algunos ocurran con mayor frecuencia que otros, lo cual genera patrones visibles o tendencias claras, como acumulaciones o desplazamientos hacia ciertas zonas, aun cuando el sistema sigue siendo aleatorio.

---

### *Caminata aleatoria con distribución no uniforme (sesgo a la derecha)*

En este caso, la caminata deja de ser completamente uniforme porque se favorece el movimiento hacia la derecha al darle mayor probabilidad que a las demás direcciones.

```js
class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    let r = random(1);

    if (r < 0.4) {
      this.x++;      // derecha (mayor probabilidad)
    } else if (r < 0.6) {
      this.x--;      // izquierda
    } else if (r < 0.8) {
      this.y++;      // abajo
    } else {
      this.y--;      // arriba
    }
  }
}
```

Con esta modificación, aunque cada paso sigue siendo aleatorio, el recorrido total del walker muestra una tendencia hacia la derecha debido a que esa dirección ocurre con mayor frecuencia. Esto demuestra cómo una distribución no uniforme puede influir en el comportamiento global del sistema sin eliminar el azar.

<img width="1507" height="667" alt="image" src="https://github.com/user-attachments/assets/f43aedea-b741-424a-98c2-9d0807626cd5" />

---

## **Actividad 4**
Aquí tienes una **explicación clara de lo que pide la actividad** y un ejemplo de **código para representar una distribución normal en p5.js**, que puedes pegar directamente en tu bitácora y usar como base para tu sketch.


### ¿Qué es una distribución normal?

Una **distribución normal** (también llamada **distribución gaussiana**) es un tipo de distribución de probabilidad donde los valores tienden a agruparse alrededor de un valor medio, y los valores lejos de ese medio son cada vez menos probables. Cuando gráficas muchos de estos números aleatorios generados de forma normal, obtienes una curva con forma de *campana* —donde la mayoría de los puntos quedan cerca del centro y pocos quedan lejos—, a diferencia de una distribución uniforme donde todos los valores son igualmente probables. ([p5js.org][1])



### Código: visualización simple de una distribución normal

 - Usando `randomGaussian()`, los mapea al eje *x*, y dibuja pequeños círculos para visualizar la forma típica de una distribución normal:

```js
let values = [];

function setup() {
  createCanvas(720, 360);
  background(255);

  // vamos a generar muchos valores y guardarlos en el arreglo
  for (let i = 0; i < 2000; i++) {
    let val = randomGaussian();      // distribución normal
    values.push(val);
  }

  noStroke();
  fill(100, 150, 255, 150);

  // dibujar cada valor como un círculo
  for (let v of values) {
    // ajustar la desviación estándar y la media
    let x = (v * 60) + width / 2;
    let y = height / 2 + random(-20, 20);
    circle(x, y, 5);
  }
}

function draw() {
  // no hacemos nada dentro de draw
}
```

* `randomGaussian()` genera valores centrados alrededor de 0, con forma de campana en la distribución. 
* Cada valor se escala y se dibuja a lo largo del eje *x* para que se vea cómo se agrupan más cerca de la media (el centro del canvas).
* Los puntos se extienden menos hacia los extremos, mostrando visualmente la probabilidad decreciente lejos del centro.
      

<img width="748" height="428" alt="image" src="https://github.com/user-attachments/assets/bf52e699-3352-42f9-9531-d154f54e341a" />


## Bitácora de aplicación 



## Bitácora de reflexión

