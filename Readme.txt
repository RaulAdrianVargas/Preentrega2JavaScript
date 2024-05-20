Desarrollo inicial del juego

// 1ro Crear personaje
	tendra un nombre que le dara el usuario
	tendra:	vida inicial
		dinero inicial
		ataque inicial
		comida inicial
		nivel 1

2do Crear array de objetos de tienda
	objetos curativos
	objetos que aumenten ataque
	objetos que llenen la barra de comida


	const objeto1 = { id:1, producto:"Arroz"};
	const array = [objeto1, {id: 2, producto:"Fideos"}];
	array.push({id: 3, producto: "Pan"}];



3ro Crear funciones escenciales
	El creador de personajes
	La tienda que podra ser llamada en la eleccion del personaje
	Selector de opciones del jugador
	Diferentes eventos aleatorios, uno en el que al viajar se encuentre con un amigo que le de comida dinero y salud; otro que se encuentre con un enemigo ( tambien random basado en el nivel y que deba decidir si ir por el camino con un enemigo u otro); uno en el que encuentra un lugar abandonado, donde tirara una moneda para ver si consigue objetos buenos o pierde vida, salud o dinero
	Opcion de descansar en el lugar, donde recuperara vida a costo de comida.
	Opcion de descansar en un hotel cercano, donde recupera vida a costo de dinero.

4to Crear un sistema de dias, en los que pase un dia despues de cada eleccion y llame de nuevo a la funcion de eleccion de opciones del jugador.

5to (si tengo tiempo crear un evento de pelea aleatorio, que luego puedo hacer que evolucione a un ataque, bloqueo, huida para posterior subida de nivel y complejizacion de eventos aleatorios)




// Declaración de objeto jugador
let personajeJugable = {
    nombre: "",
    vida: 100,
    dinero: 1000,
    ataque: 12,
    comida: 100,
    nivel: 1,
};

// Declaración de objetos de la tienda
const cura1 = { nombre: 'Pocion Chica', precio: 20 };
const cura2 = { nombre: 'Pocion Grande', precio: 50 };
const arma1 = { nombre: 'Cuchillo', precio: 100 };
const arma2 = { nombre: 'Espada', precio: 250 };
const comida1 = { nombre: 'Pan', precio: 25 };
const comida2 = { nombre: 'Asado', precio: 50 };

// Creación de array de objetos de la tienda
const objetosTienda = [cura1, cura2, arma1, arma2, comida1, comida2];

// Función para actualizar el estado del personaje
function actualizarEstado(cambioVida = 0, cambioDinero = 0, cambioComida = 0) {
    personajeJugable.vida = Math.max(0, personajeJugable.vida + cambioVida);
    personajeJugable.dinero = Math.max(0, personajeJugable.dinero + cambioDinero);
    personajeJugable.comida = Math.max(0, personajeJugable.comida + cambioComida);
}

// Función para crear el personaje
function crearPersonaje() {
    personajeJugable.nombre = prompt("Ingresar tu nombre, jugador");
}

// Función para la tienda de objetos
function tiendaDeObjetos() {
    console.log("Esta es la tienda. Estos son los productos disponibles: ");
    console.table(objetosTienda);
    const eleccion = parseInt(prompt('Ingrese el número del producto que desea comprar (1 a 6):')) - 1;

    if (eleccion >= 0 && eleccion < objetosTienda.length) {
        const producto = objetosTienda[eleccion];
        if (personajeJugable.dinero >= producto.precio) {
            console.log(`Te compraste: ${producto.nombre} y te costó: ${producto.precio} monedas`);
            actualizarEstado(0, -producto.precio, 0);
        } else {
            console.log("No tienes suficiente dinero para comprar este producto.");
        }
    } else {
        console.log("No, capo. No es gratis. Tomatela");
    }
}

// Función para eventos aleatorios
function eventosAleatorios() {
    console.log("Bueno, me temo que te tocó lo siguiente: ");
    const eventoAlAzar = Math.floor(Math.random() * 4) + 1;

    if (eventoAlAzar === 1) {
        console.log(`Te encontraste con alguien amistoso. Ganas 50 monedas, recuperas 50 puntos de vida y te da dos panes.`);
        actualizarEstado(50, 50, 40);
    } else if (eventoAlAzar === 2) {
        console.log("Te encontraste con un enemigo. Pierdes 100 de vida y te roba 100 monedas.");
        actualizarEstado(-100, -100, 0);
    } else if (eventoAlAzar === 3) {
        console.log("Pasaste por un puesto callejero e hiciste una apuesta. Jugaste cara o cruz.");
        jugarCaraCruz();
    } else {
        console.log("No se cómo hiciste, pero sos el rey de los gilazos");
    }
}

// Función para jugar cara o cruz
function jugarCaraCruz() {
    const eleccionJugador = prompt("Elija 'cara' o 'cruz'.").trim().toLowerCase();

    if (eleccionJugador !== 'cara' && eleccionJugador !== 'cruz') {
        console.log("Amigo, cara o cruz, no es tan complicado.");
        return;
    }

    const resultadoMoneda = lanzarMoneda();
    console.log(`La moneda cayó en ${resultadoMoneda}`);
    if (eleccionJugador === resultadoMoneda) {
        console.log("¡Felicidades! Ganaste.");
        actualizarEstado(0, 200, 0);
    } else {
        console.log("No amigo, tenés una sal.");
        actualizarEstado(0, -200, 0);
    }
}

// Función para lanzar una moneda
function lanzarMoneda() {
    return Math.random() < 0.5 ? "cara" : "cruz";
}

// Función para descansar en el lugar
function mimirAcaNomas() {
    console.log("Decidís descansar en donde estás. Te comes algo tranqui y recuperas vida.");
    actualizarEstado(100, 0, -20);
}

// Función para descansar en un hotel
function mimirEnHotel() {
    console.log("Decidís descansar en un hotel cercano. Es cómodo y calentito. Recuperas vida.");
    actualizarEstado(250, -150, 0);
}

// Función para manejar la elección del jugador
function eleccionJugador() {
    console.log("¿Qué deseas hacer en este momento?");

    let respuesta = parseInt(prompt("1. Descansar un rato, 2. Ir a una tienda, 3. Aventurarse a lo desconocido, 4. Buscar un hotel"));

    if (respuesta === 1) {
        mimirAcaNomas();
    } else if (respuesta === 2) {
        tiendaDeObjetos();
    } else if (respuesta === 3) {
        eventosAleatorios();
    } else if (respuesta === 4) {
        mimirEnHotel();
    } else {
        console.log("No capo. Sos muy boludo vos.");
    }
}

// Función para mostrar el estado del personaje
function mostrarEstatus() {
    const respuesta = confirm("¿Quieres ver tu estado actual?");
    if (respuesta) {
        console.table(personajeJugable);
    } else {
        console.log("Joya. ¿Entonces qué hacemos?");
    }
}

// Función para continuar el juego
function continuar() {
    let seguirJugando = true;

    while (seguirJugando) {
        const respuesta = confirm("¿Quieres continuar la aventura?");
        if (respuesta) {
            eleccionJugador();
            mostrarEstatus();
        } else {
            console.log("Gracias por jugar :D");
            console.log("Has finalizado tu aventura");
            seguirJugando = false;
        }
    }
}

//-----------------------------------------------------------------------------------
// Aquí inicializa el juego

crearPersonaje();

console.table(personajeJugable);

console.log("Bienvenido al juego RPG de JavaScript " + personajeJugable.nombre);

eleccionJugador();

continuar();




//----------------------------------------------------------

// Declaracion de objeto jugador
let personajeJugable = {

    nombre: "",
    vida: 100,
    dinero: 1000,
    ataque: 12,
    comida: 100,
    nivel: 1,

}

// Declaracion de objetos de la tienda

const cura1= {nombre: 'Pocion Chica', precio: 20};
const cura2= {nombre: 'Pocion Grande', precio: 50};
const arma1= {nombre: 'Cuchillo', precio: 100};
const arma2= {nombre: 'Espada', precio: 250};
const comida1= {nombre: 'Pan', precio: 25};
const comida2= {nombre: 'Asado', precio: 50};

// Creacion de array de objetos de la tienda

const objetosTienda =[]
objetosTienda.push(cura1);
objetosTienda.push(cura2);
objetosTienda.push(arma1);
objetosTienda.push(arma2);
objetosTienda.push(comida1);
objetosTienda.push(comida2);

function actualizarEstado(cambioVida = 0, cambioDinero = 0, cambioComida = 0) {
    personajeJugable.vida = Math.max(0, personajeJugable.vida + cambioVida);
    personajeJugable.dinero = Math.max(0, personajeJugable.dinero + cambioDinero);
    personajeJugable.comida = Math.max(0, personajeJugable.comida + cambioComida);
}

// Comienzo a crear funciones basicas del juego
function crearPersonaje(){
    personajeJugable.nombre= prompt("Ingresar tu nombre, jugador");
}

function tiendaDeObjetos(){
    console.log("Esta es la tienda. Estos son los productos disponibles: ");
    console.table(objetosTienda)
    const eleccion= parseInt(prompt('Ingrese el numero del producto que desea comprar (1 a 6):'))-1;

    if(eleccion >= 0 && eleccion < objetosTienda.length){
        const producto= objetosTienda[eleccion];
        console.log(`Te compraste: ${producto.nombre} y te salió: ${producto.precio} monedas`)
        actualizarEstado(0, -producto.precio, 0);
    }
    else{
        console.log("No, capo. No es gratis. Tomatela")
    }
}

function eventosAleatorios(){

    console.log("Bueno, me temo que te toco lo siguiente: ")
    const eventoAlAzar= Math.floor(Math.random()*4) + 1;

    if (eventoAlAzar === 1){
        console.log(`Te encontraste con alguien amistoso. Ganas 50 monedas, recuperas 50 puntos de vida y te da dos panes.`)
        actualizarEstado(50,50,0)
    }
    else if(eventoAlAzar === 2){
        console.log("Te encontraste con un enemigo. Perdes 100 de vida y te roba 100 monedas.")
        actualizarEstado(-100,-100,0)
    }
    else if(eventoAlAzar === 3){
        console.log("Pasaste por un puesto callejero e hiciste una apuesta. Jugaste cara o cruz ")
        function jugarCaraCruz(){
            const eleccionJugador = prompt("Elija 'cara' o 'cruz'.").trim().toLowerCase();
            
            if (eleccionJugador !== 'cara' && eleccionJugador !== 'cruz'){
                console.log("Amigo, cara o cruz, no es tan complicado.")
                return
            }

            const resultadoMoneda = lanzarMoneda();
            console.log(`La moneda cayo en ${resultadoMoneda}`);
            if (eleccionJugador === resultadoMoneda){
                console.log("!Felicidades! Ganaste.");
                actualizarEstado(0,200,0)
            }
            else{
                console.log("No amigo, tenes una sal.")
                actualizarEstado(0,-200,0)
            }
        }
        jugarCaraCruz()
    }
    else{
        console.log("No se como hiciste, pero sos el rey de los gilazos")
    }
}

function lanzarMoneda(){
    const resultado=Math.random() < 0.5 ? "cara" : "cruz";
    return resultado
}

function mimirAcaNomas(){
    console.log("Decidis descansar en donde estas. Te comes algo tranqui y recuperas vida.")
    actualizarEstado(50,0,-20)
}

function mimirEnHotel(){
    console.log("Decidis descansar en un hotel cercano. Es comodo y calentito. Recuperas vida.")
    actualizarEstado(250,-150,0)
}



function eleccionJugador(){
    console.log("¿Que deseas hacer en este momento?");

    let respuesta =parseInt(prompt("1. Descansar un rato, 2. Ir a una tienda, 3. Aventurarse a lo desconocido, 4. Buscar un hotel"))

    if(respuesta === 1){
        mimirAcaNomas();
    }
    else if(respuesta === 2){
        tiendaDeObjetos();
    }
    else if(respuesta === 3){
        eventosAleatorios();
    }
    else if(respuesta === 4){
        mimirEnHotel();
    }
    else{
        console.log("No capo. Sos muy boludo vos")
    }
}

function mostrarEstatus(){
    const respuesta= confirm("¿Quieres ver tu estado actual?")
    if(respuesta){
        console.table(personajeJugable)
    }
    else{
        console.log("Joya. ¿Entonces que hacemos?")
    }
}

function continuar(){
    let seguirJugando= true;

    while(seguirJugando){
    const respuesta= confirm("¿Quieres continuar la aventura?")
    if(respuesta){
        eleccionJugador()
        mostrarEstatus()
    }
    else{
        console.log("Gracias por jugar :D")
        console.log("Has finalizado tu aventura")
        seguirJugando=false
    }
}
}


//-----------------------------------------------------------------------------------
// Aca inicializa el juego

crearPersonaje();

console.table(personajeJugable);

console.log("Bienvenido al juego RPG de javascript " + personajeJugable.nombre);

eleccionJugador();

continuar();

//-------------------------------------------------------------------------
// Declaracion de objeto jugador
let personajeJugable = {
    nombre: "",
    vida: 100,
    dinero: 1000,
    ataque: 12,
    comida: 100,
    nivel: 1,
    mochila: []
};

// Función para añadir un producto a la mochila
function agregarAMochila(producto) {
    personajeJugable.mochila.push(producto);
}

// Actualizar estado considerando la mochila
function actualizarEstado(cambioVida = 0, cambioDinero = 0, cambioComida = 0, productoComprado = null) {
    personajeJugable.vida = Math.max(0, personajeJugable.vida + cambioVida);
    personajeJugable.dinero = Math.max(0, personajeJugable.dinero + cambioDinero);
    personajeJugable.comida = Math.max(0, personajeJugable.comida + cambioComida);

    if (productoComprado) {
        agregarAMochila(productoComprado);
        console.log(`Has añadido ${productoComprado.nombre} a tu mochila.`);
    }

    // Mensajes adicionales según el tipo de producto o acción
    if (cambioVida > 0) {
        console.log(`Recuperaste ${cambioVida} puntos de vida.`);
    }
}

// En la función tiendaDeObjetos, al comprar un producto:
if (eleccion >= 0 && eleccion < productosFiltrados.length) {
    const producto = productosFiltrados[eleccion];
    if (personajeJugable.dinero >= producto.precio) {
        console.log(`Te compraste: ${producto.nombre} y te salió: ${producto.precio} monedas`);
        
        // Actualizar estado según el tipo de producto
        if (producto.tipo === 'cura') {
            actualizarEstado(50, -producto.precio, 0, producto); // Ajusta el cambio de vida según lo que recupere la cura
        } else {
            actualizarEstado(0, -producto.precio, 0, producto);
        }
    } else {
        console.log("No tienes suficiente dinero para comprar este producto.");
    }
}

