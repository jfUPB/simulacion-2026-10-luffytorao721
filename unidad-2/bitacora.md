# Unidad 2

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

https://editor.p5js.org/luffytorao721/sketches/6MmYTcpT7


# Concepto de la obra generativa

Esta obra explora la idea de **atracción y transformación visual**.

Las partículas se comportan como organismos que sienten una fuerza de atracción hacia el mouse. El mouse representa un punto de energía que altera el entorno. Sin embargo, aunque el comportamiento físico es el mismo, la identidad visual cambia con cada click.

La pieza propone una reflexión sobre cómo:

* La **misma estructura interna (reglas físicas)**
* Puede producir **identidades visuales completamente distintas**

Es una exploración artística de cómo el comportamiento (movimiento) y la apariencia (forma y color) pueden separarse.

---

# Regla aplicada para la aceleración

La regla principal es:

> La aceleración es un vector que apunta hacia el mouse, con una pequeña perturbación aleatoria.

Matemáticamente:

```
aceleración = dirección normalizada hacia el mouse * 0.2 + ruido pequeño
```

### ¿Por qué esta decisión?

Fue una decisión de diseño porque:

* La aceleración hacia el mouse genera cohesión visual.
* El ruido agrega organicidad.
* El movimiento nunca es perfectamente predecible.
* Produce trayectorias vivas y dinámicas.

En lugar de programar directamente la posición, se define la aceleración y se deja que el sistema evolucione usando el marco **Motion 101**:

1. velocity += acceleration
2. position += velocity

Esto permite que el comportamiento emerja naturalmente.

---

# Cómo funciona el código

## Sistema de partículas

Se crea un arreglo:

```js
let particles = [];
```

En `setup()` se instancian 80 partículas:

```js
particles.push(new Particle());
```

---

##  Motion 101

En cada frame (`draw()`):

```js
this.velocity.add(this.acceleration);
this.velocity.limit(this.maxSpeed);
this.position.add(this.velocity);
```

Interpretación geométrica:

* La aceleración cambia la velocidad.
* La velocidad cambia la posición.
* El movimiento es acumulativo.

---

##  Interactividad (click)

Cada vez que se hace click:

```js
mode++;
```

Luego usamos:

```js
mode % 4
```

Esto permite alternar entre 4 estilos visuales diferentes.

---

##  Modos visuales

| Modo | Forma               | Tamaño | Color                    |
| ---- | ------------------- | ------ | ------------------------ |
| 0    | Círculo pequeño     | 5 px   | Blanco                   |
| 1    | Círculo grande      | 12 px  | Gradiente según posición |
| 2    | Triángulo orientado | Medio  | Naranja                  |
| 3    | Cuadrado rotando    | Grande | Azul                     |

El movimiento no cambia.
Solo cambia la representación visual.

Esto demuestra separación entre:

* Comportamiento (física)
* Estética (visualización)

---

# Código completo de la aplicación

```javascript
let particles = [];
let mode = 0;

function setup() {
  createCanvas(600, 600);
  
  for (let i = 0; i < 80; i++) {
    particles.push(new Particle());
  }
  
  background(10);
}

function draw() {
  fill(10, 40);
  noStroke();
  rect(0, 0, width, height);
  
  for (let p of particles) {
    p.update();
    p.display();
  }
}

function mousePressed() {
  mode++;
}

class Particle {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxSpeed = 4;
  }
  
  update() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    
    dir.normalize();
    dir.mult(0.2);
    
    let noise = p5.Vector.random2D();
    noise.mult(0.05);
    
    this.acceleration = p5.Vector.add(dir, noise);
    
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
  }
  
  display() {
    let currentMode = mode % 4;
    
    push();
    translate(this.position.x, this.position.y);
    
    if (currentMode === 0) {
      stroke(255);
      fill(255, 150);
      ellipse(0, 0, 5);
      
    } else if (currentMode === 1) {
      let c = map(this.position.x, 0, width, 0, 255);
      fill(c, 100, 255 - c);
      noStroke();
      ellipse(0, 0, 12);
      
    } else if (currentMode === 2) {
      fill(255, 100, 0);
      noStroke();
      rotate(this.velocity.heading());
      triangle(-6, 6, 6, 6, 0, -6);
      
    } else if (currentMode === 3) {
      stroke(0, 200, 255);
      noFill();
      rectMode(CENTER);
      rotate(frameCount * 0.02);
      rect(0, 0, 15, 15);
    }
    
    pop();
  }
}
```



## Bitácora de reflexión
