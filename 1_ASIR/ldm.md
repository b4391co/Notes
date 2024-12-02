<h1 style="text-align:center;font-size:40px;">LDM</h1>

## HTML

CSSstub

```css
<style>
## class col dentro de class row en div cuya clase empieza por col* .row>.col,div[class^="col"]{
    **    **padding-top: .75rem;
    **    **padding-bottom: .75rem;
    **    **background-color: rgba(39, 41, 43, 0.03);
    **    **border: 1px solid rgba(28, 28, 29, 0.1)**    **;
    border-radius: 10px;
    }
</style>
```

## JS

```
<script src="main.js"></script>
```

### script:

```
let test = “Test” // var numero = 12 # Variable let o var
const COLOR_ROJO = “#FF0000” # Constante
alert (“Hola” + test + numero) # alerta muestra Hola + variable test + var numero
console.log(*variable*) # muestra por consola la variable que se ponga
```

### recoge elemento contenido por id

```
const contenido = document.getElementById("contenido")
contenido.innerHTML="Hola" # pone "hola" en el div con ID = contenido
```

### cambiar contenido desde JS a HTML, introduce divs

```
contenido.innerHTML="<div>"+nombre+"</div>"; == `<div>$ {nombre}</div>`
contenido.innerHTML +="<div>"+edad+"</div>"; # Concatenar
```

### Condiciones

#### if

```
var condicion=21;
var condicio2=22;
if(condicion=21) {
alert("Correcto");
}else {
alert("falso");
}elseif(condicio2=22) {
alert("tmb correcto");
}
```

#### for

```
for(var i = 0; i <= 10; i++){ # i = x; mientras i <= 10; i=i+1 // cada ejecución
contenido.innerHTML +=`<div>Edad: ${i} años</div>`;
}
```

#### Functions

```html
function addContent(newContent){ contenido.innerHTML += newContent; }
addContent("Holaa");
<!--  al ejecutar addContent Holaa es el newContent y se inserta -->
```

#### Arrays

```
var edades = [23, 45, 2, 42];
console.log(array[3]);
> Juntar con bucles
for(var i = 0; i < edades.length; i++){
addContent(`<div>Edad: ${edades[i]} años</div>`);
}
```

### Botón

```html
<button class="btn" id="boton">Ajax</button> # con el enlace a JS
document.querySelector('#boton').addEventListener('click',traerDatos); function
traerDatos(){ console.log('funcion activada'); }
```

### [AJAX](https://www.w3schools.com/js/js_ajax_intro.asp)

```
function traerDatos(){
    const xhttp = new XMLHttpRequest();
    xhttp.open('GET','text.txt', true);
    xhttp.send();
    xhttp.onreadystatechange = function(){
        if(this.readyState == 4 && this.status == 200){ ESTADOS
        console.log(this.responseText);
        }
    }
}
```

### [BOOTSTRAP](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

```html
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- Bootstrap CSS -->
<link
href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/css/bootstrap.min.css" rel="stylesheet"
integrity="sha384-wEmeIV1mKuiNpC+IOBjI7aAzPcEZeedi5yW5f2yOq55WWLwNGmvvx4Um1vskeMj0" crossorigin="anonymous">
...
</head>
<body>
...
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-p34f1UUtsS3wqzfto5wAAmdvj+osOnFyQFpp4Ua3gs/ZVWx6oOypYoCJh GGScy+8" crossorigin="anonymous"></script>
</body>
```
