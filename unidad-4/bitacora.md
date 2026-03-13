# Unidad 4

## Bitácora de proceso de aprendizaje

# Actividad 1

Sobre la obra de **Memo Akten**

**Bitácora (ejemplo):**

La obra me llamó la atención porque usa matemáticas y movimiento para crear arte visual. Los elementos se mueven de forma armónica y parecen seguir patrones naturales como ondas o oscilaciones. Me gustó cómo el movimiento genera una sensación orgánica e interactiva.

---

# Actividad 2

### ¿Qué pasa en la simulación?

Los objetos **rotan usando un ángulo que cambia en cada frame**.

### ¿Cuál es la interacción?

El objeto gira alrededor del **centro del sistema de coordenadas**.

### ¿Por qué mover el origen al centro?

Para que la **rotación ocurra alrededor del centro de la pantalla**.

### Relación entre sistema de coordenadas y `rotate()`

`rotate()` **rota todo el sistema de coordenadas**, no solo el objeto.

### ¿Por qué dibujar en (0,0)?

Porque al rotar el sistema de coordenadas, **todo lo que está en (0,0) rota automáticamente**.

### ¿Por qué los elementos rotan si el código es el mismo?

Porque **el ángulo cambia cada frame**.

---

### Segunda simulación

### ¿Qué hace Motion 101?

Actualiza:

* **posición**
* **velocidad**
* **aceleración**

### ¿Qué hace `heading()`?

Devuelve el **ángulo del vector de velocidad**.

### ¿Qué hace `push()` y `pop()`?

Guardan y restauran **transformaciones** para que no afecten otros objetos.

### ¿Qué hace `rectMode(CENTER)`?

Dibuja el rectángulo **desde el centro**.

### Relación velocidad y rotación

El objeto **rota en la dirección de su velocidad**.

---

# Actividad 3

Vehículo triangular que se mueve con flechas:

```javascript
let pos;
let vel;

function setup(){
createCanvas(600,400);
pos=createVector(width/2,height/2);
vel=createVector(0,0);
}

function draw(){
background(255);

if(keyIsDown(LEFT_ARROW)){
vel.x-=0.1;
}

if(keyIsDown(RIGHT_ARROW)){
vel.x+=0.1;
}

pos.add(vel);

let angle=vel.heading();

push();
translate(pos.x,pos.y);
rotate(angle);

triangle(-15,10,-15,-10,15,0);

pop();
}
```

---

# Actividad 4

### Motion 101 con fuerzas

Se agrega:

```javascript
acceleration.add(force)
```

Las **fuerzas se acumulan**.

### Attractor

Es el objeto que **atrae a los demás con una fuerza**.

### Cambiar color

Modificar:

```javascript
fill(255,0,0)
```

### Mover con el mouse

```javascript
function mouseDragged(){
attractor.position.x=mouseX;
attractor.position.y=mouseY;
}
```

---

# Actividad 5

### Relación coordenadas polares

```
x = r * cos(theta)
y = r * sin(theta)
```

### ¿Qué ocurre con `fromAngle(theta)`?

Crea un **vector de longitud 1** en ese ángulo.

### ¿Qué ocurre con `fromAngle(theta,r)`?

Crea un **vector con magnitud r**, por eso el punto gira con **radio r**.

---

# Actividad 6

Conceptos:

* **Amplitud:** altura de la onda
* **Frecuencia:** qué tan rápido oscila
* **Periodo:** tiempo de un ciclo
* **Fase:** desplazamiento de la onda

Ejemplo simple:

```javascript
let angle=0;

function draw(){
background(255);

let y=sin(angle)*100;

ellipse(width/2,height/2+y,20,20);

angle+=0.05;
}
```

---

# Actividad 7

Modificar simulación agregando:

**Aleatoriedad**

```javascript
velocity.x += noise(frameCount)*0.1;
```

**Fuerza**

```javascript
let gravity=createVector(0,0.1);
acceleration.add(gravity);
```

---

# Actividad 8

Mover la onda:

```javascript
let angle=0;

function draw(){
background(255);

for(let x=0;x<width;x+=20){

let y=sin(angle+x*0.05)*50;

ellipse(x,height/2+y,10,10);
}

angle+=0.05;
}
```

La onda se mueve porque **el ángulo cambia continuamente**.

---

# Actividad 9

Dos resortes en serie:

Concepto:

* masa 1 conectada al punto fijo
* masa 2 conectada a masa 1

Fuerza del resorte:

```
F = -k * x
```


## Bitácora de aplicación 
### https://editor.p5js.org/luffytorao721/sketches/yTbzR_Sb3

Este proyecto es una obra generativa interactiva que combina movimiento, sonido y visualización dinámica, basada en los conceptos aprendidos en las unidades anteriores y en la unidad actual de movimiento angular y oscilatorio.

Objetivo

Crear un sistema de partículas-notas musicales que:

Se muevan de manera armónica alrededor de un punto central.

Reaccionen a la interacción del usuario (mouse y teclado).

Generen sonido y colores dinámicos que sigan el ritmo del movimiento.

Dejen un rastro visual (estelas) que resalte el movimiento y la interacción.


```Js
let notes = [];
let bubbles = [];
let center;
let globalAngle = 0;
let mode = 0;

let started = false;

function setup() {
  createCanvas(800, 600);
  textAlign(CENTER, CENTER);

  center = createVector(width / 2, height / 2);

  // Crear notas
  for (let i = 0; i < 60; i++) {
    notes.push(new MusicalNote());
  }

  // Crear burbujas del fondo tipo soda
  for (let i = 0; i < 80; i++) {
    bubbles.push(new Bubble());
  }
}

function draw() {
  // Fondo efervescente tipo soda
  fill(0, 50, 100, 60); // azul transparente
  noStroke();
  rect(0, 0, width, height);

  // Dibujar burbujas del fondo
  for (let b of bubbles) {
    b.update();
    b.display();
  }

  globalAngle += 0.03;

  // Centro
  fill(255, 200, 0);
  noStroke();
  circle(center.x, center.y, 14);

  for (let n of notes) {
    n.applyCenterForce();
    n.applyWave();
    n.mouseForce();
    n.update();
    n.checkSound();
    n.display();
  }
}

// Activar audio y explotar partículas
function mousePressed() {
  if (!started) {
    userStartAudio();
    started = true;
  }

  for (let n of notes) {
    let dir = p5.Vector.sub(n.pos, createVector(mouseX, mouseY));
    dir.normalize();
    dir.mult(4);
    n.vel.add(dir);
  }
}

// Clase nota musical
class MusicalNote {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);

    this.symbol = random(["♪", "♫", "♬", "♩"]);
    this.baseSize = random(20, 40);
    this.phase = random(TWO_PI);

    this.freq = random([220, 261, 293, 329, 349, 392, 440]);

    this.osc = new p5.Oscillator();
    this.osc.setType("sine");
    this.osc.freq(this.freq);
    this.osc.amp(0);
    this.osc.start();

    this.playing = false;
  }

  applyCenterForce() {
    let dir = p5.Vector.sub(center, this.pos);
    dir.normalize();
    dir.mult(0.05);
    this.acc.add(dir);
  }

  applyWave() {
    let force;
    if (mode == 0) force = createVector(sin(globalAngle + this.phase) * 0.2, 0);
    if (mode == 1) force = createVector(0, cos(globalAngle + this.phase) * 0.2);
    if (mode == 2) force = p5.Vector.fromAngle(globalAngle + this.phase, 0.25);
    this.acc.add(force);
  }

  mouseForce() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.pos);
    let d = dir.mag();
    if (d < 160) {
      dir.normalize();
      dir.mult(map(d, 0, 160, 0.6, 0));
      this.acc.add(dir);
    }
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(3);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  checkSound() {
    let d = dist(this.pos.x, this.pos.y, center.x, center.y);
    if (d < 60 && !this.playing) {
      this.osc.amp(0.3, 0.05);
      this.playing = true;
    }
    if (d >= 60 && this.playing) {
      this.osc.amp(0, 0.2);
      this.playing = false;
    }
  }

  display() {
    let r = sin(globalAngle + this.pos.x * 0.01) * 127 + 128;
    let g = sin(globalAngle + this.pos.y * 0.01) * 127 + 128;
    let b = sin(globalAngle) * 127 + 128;
    fill(r, g, b);

    let size = this.baseSize + sin(globalAngle + this.phase) * 10;
    textSize(size);
    text(this.symbol, this.pos.x, this.pos.y);
  }
}

// Clase burbujas del fondo
class Bubble {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.size = random(5, 20);
    this.speed = random(0.5, 2);
    this.color = color(255, 255, 255, random(50, 150));
  }

  update() {
    this.pos.y -= this.speed;
    if (this.pos.y < -this.size) this.pos.y = height + this.size;
  }

  display() {
    fill(this.color);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

function keyPressed() {
  if (key == " ") mode = (mode + 1) % 3;
  if (key == "A" || key == "a") notes.push(new MusicalNote());
}
```

https://editor.p5js.org/luffytorao721/sketches/i8PJtN8w5 ejemplo con musica descargada

## Bitácora de reflexión

### Unidad 1 – Fundamentos

Variables y tipos de datos

Aleatoriedad y selección aleatoria


### Unidad 2 – Motion 101

Vectores: posición, velocidad, aceleración

Update loop: pos += vel, vel += acc

Movimiento básico (lineal y continuo)

### Unidad 3 – Fuerzas

Aceleración por fuerzas externas

Atracción, repulsión

Interacción con usuario (mouse, teclado)

### Unidad 4 – Movimiento angular y oscilatorio

Movimiento armónico simple (sin, cos)

Ángulos y coordenadas polares

Ondas y resortes

Sonido interactivo (p5.sound)

Sincronización visual y sonora
