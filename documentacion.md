# Documentación del Proyecto

# Proyecto Interfaces

## **Carátula**

**Título del Proyecto:** Proyecto Interfaces  

**Alumno:** Carlos calderon Rodriguez

**Curso:** DAM (Desarrollo de Aplicaciones Multiplataforma)

## **Introducción**

MiWeb es una aplicación web que proporciona soluciones digitales para el análisis de datos y desarrollo web. Incluye una interfaz con navegación, un modo oscuro, un reloj en tiempo real y una tabla dinámica que obtiene datos de una API externa,y nos da un grafico de datos apartir de una pi externa.

## **Tecnologias utilizadas**

HTML5 - Estructura de la aplicación.

CSS3 + Bootstrap - Diseño y estilos responsivos.

JavaScript - Funcionalidad dinámica de la página.

JSONPlaceholder API - Fuente de datos para la tabla dinámica.

## **JavaScript principales**

1. **Color oscuro**

Con esto hacemos que la pagina web cambie de modo claro que esta a oscuro


```java
//Se asegura que el contenido del DOM esté completamente cargado antes de ejecutar el código.
document.addEventListener("DOMContentLoaded", () => {
    
    //Se obtiene el botón para cambiar entre modo claro y oscuro.
    const toggleButton = document.getElementById("darkModeToggle");
    
    //Se obtiene el elemento <body> para poder modificar su clase.
    const body = document.body;

    //Verifica si en el almacenamiento local (localStorage) está guardada la preferencia de tema como 'dark'.
    if (localStorage.getItem("theme") === "dark") {
        // Si es 'dark', agrega la clase 'dark-mode' al <body> para aplicar el modo oscuro.
        body.classList.add("dark-mode");
    }

    //Se agrega un escuchador de eventos al botón para cambiar entre los modos.
    toggleButton.addEventListener("click", () => {
        
        //Se alterna la clase 'dark-mode' en el <body> al hacer clic en el botón.
        body.classList.toggle("dark-mode");

        //Si el <body> contiene la clase 'dark-mode', se guarda en localStorage que el tema es oscuro.
        if (body.classList.contains("dark-mode")) {
            localStorage.setItem("theme", "dark");
        } else {
            //Si no tiene la clase 'dark-mode', se guarda que el tema es claro.
            localStorage.setItem("theme", "light");
        }
    });
});
 ```

2. **Reloj digital**

Con esto hacemos que se muestre en el menu un reloj digital a tiempo real


```java
function updateClock() {
    //Se obtiene el elemento con el ID 'clock' para actualizar su contenido
    const clock = document.getElementById("clock");
    
    //Se crea una nueva instancia de Date, que obtiene la fecha y hora actual
    const now = new Date();
    
    //Se obtiene la hora actual, se convierte a string y se asegura que tenga dos dígitos
    const hours = String(now.getHours()).padStart(2, "0");
    const minutes = String(now.getMinutes()).padStart(2, "0");
    const seconds = String(now.getSeconds()).padStart(2, "0");
    
    // Se actualiza el contenido de la etiqueta con el ID 'clock' para mostrar la hora
    clock.textContent = `${hours}:${minutes}:${seconds}`;
}

//Se configura un intervalo para ejecutar la función 'updateClock' cada 1000 milisegundos (1 segundo)
setInterval(updateClock, 1000);

//Se asegura de que 'updateClock' se ejecute tan pronto como se haya cargado el contenido HTML
document.addEventListener("DOMContentLoaded", updateClock);

 ```

3. **Cambiar color a una tabla de datos**

Con esto hacemos que varie de colores la tabla de datos que hemos creado para darle una funcionalidad


```java
//Se obtiene el elemento con el ID 'colorSelector' (presumiblemente un selector de color)
const colorSelector = document.getElementById("colorSelector");

//Se obtiene la tabla que está dentro de un contenedor con la clase 'table-container'
const table = document.querySelector("table");

//Se agrega un escuchador de eventos al selector de color para que se ejecute cuando el usuario elija un color
colorSelector.addEventListener("input", (event) => {
    
    // Cuando el usuario cambia el color, se actualiza el color de fondo de la tabla
    // 'event.target' se refiere al elemento que disparó el evento, que en este caso es el 'colorSelector'
    // 'event.target.value' contiene el color seleccionado por el usuario
    // Este color se aplica como el nuevo color de fondo de la tabla.
    table.style.backgroundColor = event.target.value;
});
 ```


## **Llamadas a Apis desde JavaScript**

1. **Añadir datos de una Api externa**

Aqui lo que hacemos es obtener los datos de la api para representarlos en la tabla


```java

document.addEventListener("DOMContentLoaded", () => {
    
    //Se obtiene el elemento <tbody> de la tabla con el id 'dataTable'
    const tableBody = document.querySelector("#dataTable tbody");

    //Llamada a la API utilizando fetch para obtener los datos de usuarios.
    fetch("https://jsonplaceholder.typicode.com/users")
        .then(response => response.json()) //Convertir la respuesta a formato JSON
        .then(users => {
            tableBody.innerHTML = ""; //Limpiar cualquier contenido previo en el cuerpo de la tabla.

            // Recorremos la lista de usuarios obtenidos de la API.
            users.forEach(user => {
                //Crear una nueva fila de tabla (<tr>) para cada usuario.
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>${user.id}</td>
                    <td>${user.name}</td>
                    <td>${user.email}</td>
                    <td>${user.phone}</td>
                `; //Insertar los datos del usuario en las celdas de la fila

                // 9. Agregar la fila de la tabla al <tbody> previamente seleccionado.
                tableBody.appendChild(row);
            });
        })
        .catch(error => console.error("Error al obtener los datos:", error)); //Captura errores en caso de que la llamada a la API falle
});
 ```
2. **Añadir Imagenes de una Api externa**

Aqui lo que hacemos es obtener imagenes de una api 


```java


document.addEventListener("DOMContentLoaded", () => {
    const galleryContainer = document.getElementById("imageGallery");

    // Generar un número de página aleatorio (por ejemplo, entre 1 y 50)
    const randomPage = Math.floor(Math.random() * 50) + 1;

    // Llamamos a la API de Picsum Photos con la página aleatoria
    fetch(`https://picsum.photos/v2/list?page=${randomPage}&limit=6`)
        .then(response => response.json())
        .then(images => {
            galleryContainer.innerHTML = ""; // Limpiar el contenedor

            images.forEach(image => {
                const imgCol = document.createElement("div");
                imgCol.classList.add("col-md-4", "mb-3");

                // Usamos un tamaño específico para las imágenes (por ejemplo, 300x200)
                const imageUrl = `https://picsum.photos/id/${image.id}/300/200`;

                imgCol.innerHTML = `
                    <div class="card">
                        <img src="${imageUrl}" class="card-img-top" alt="Foto por ${image.author}" loading="lazy">
                        
                    </div>
                `;
                galleryContainer.appendChild(imgCol);
            });
        })
        .catch(error => console.error("Error al obtener las imágenes:", error));
});


 ```
 3. **Obtener datos de una api para el grafico**

Aqui lo que hacemos es obtener los usuarios que tiene la api para representarlos en un grafico


```java


document.addEventListener("DOMContentLoaded", () => {
    // Usamos la misma API para obtener los usuarios
    fetch("https://jsonplaceholder.typicode.com/users")
        .then(response => response.json())
        .then(users => {
            // Preparamos los datos para el gráfico
            // Etiquetas: el nombre de cada usuario
            const labels = [];
            for (let i = 0; i < users.length; i++) {
                labels.push(users[i].name);
            }
            console.log(labels);
            // Datos: cantidad de caracteres en cada nombre (puedes cambiar esta métrica)
            const dataValues = users.map(user => user.name.length);

            // Obtenemos el contexto del canvas
            const ctx = document.getElementById("usersChart").getContext("2d");

            // Creamos el gráfico de barras
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Cantidad de caracteres en el nombre',
                        data: dataValues,
                        backgroundColor: 'rgba(35, 119, 101, 0.5)',
                        borderWidth: 1
                    }]
                },
                
            });
        })
        
});


 ```

## **Css relevantes**

 1. **Para el modo oscuro cambiar algunas especificaciones**

 Se añade el dark-mode para que cambie de color al momento que cambie en el javascript,de modo hacemos que en el body se haga en el css las clases que tengan dark-mode
 ```java


body.dark-mode {
  background: linear-gradient(
    to bottom,
    rgb(34, 34, 34),
    100%,
    rgb(48, 105, 92)
  );
  color: #ffffff;
}

body.dark-mode .navbar {
  background-color: #181818;
  color: #ffffff;
}

body.dark-mode footer {
  background-color: #2c2f33;
  color: #ffffff;
}

body.dark-mode .service-icon {
  color: #ffffff;
}


 ```

## **Pruebas con Axe DevTools**
### Ventana 1
![Captura 1](capturas/paginaweb1.png "Vista 1")
### Ventana 2
![Captura 2](capturas/paginaweb2.png "Vista 2")

## **Contacto**

Para más información, contáctanos en soporte@miweb.com