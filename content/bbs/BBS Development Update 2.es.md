+++
title = "Actualización #2 del estado de desarrollo de Skycoin BBS"
tags = [
    "Development",
    "BBS",
    "CXO",
]
date = "2017-08-05"
categories = [
    "Development Updates",
]
description = "La segunda actualización de desarrollo para Skycoin BBS."
+++

Ha pasado poco más de un mes desde la publicación de la versión 0.1, y la versión 0.2 pronto estará lista!

## Cambios CXO

CXO ha sido reestructurado para ser más rápido y estable. Se ha hecho que la API funcione mejor con **hash arrays** - con un tiempo  constante de acceso, replicación más rápida y la posibilidad para un determinado hash acceder directamente al elemento.

[Mira el repositorio de CXO aqui](https://github.com/skycoin/cxo)

## Cambios en la estrucura de CXO 

Los cambios en la estructura son para solucionar los siguientes problemas:

1. Implementación de una estructura que permita en el futuro establecer unos directorios separados para los datos del usuario.
2. Determinar fácilmente los cambios en la estructura de carpetas. Lo que será útil para compilar vistas y ofrecer al usuario final actualizaciones en tiempo real.
3. Determinar facilmente el tipo de **objetos raiz** para las diferentes **ramas**(directorios base?)

![Skycoin BBS v0.2 CXO Datastructure](/bbs/img/bbs_cxo_datastructure_v0.2.png)

El objeto PaginaRaíz (RootPage) determina el tipo de raíz. De momento, todo los datos se almacenan bajo un solo árbol raíz por tablón. En el futuro, hilos y usuarios tendrán sus raices individuales.

En el futuro, la PaginaTablón (*BoardPage*) tendrá una lista de llaves públicas en vez de **referncias** (href) por hilos, como los hilos tendrán sus propias raices. Esto hace que los hilos se puedan traspasar fácilmente entre tablones.
 
Diff page se usa para determinar los cambios entre los directorios base para la BoardPage base.** Esto es en esencia un conjunto de arrays siempre crecientes, donde un incremento de la longitud  se interpreta como cambios.

La página de los usuarios (UsersPage) se convertirá en una liste de claves públicas (serían como "participantes" en un tablón)- Cada usuarió tendrá su propia raíz.

## Implementación de los módulos de las vistas.

Una vista es en esencia una iterfaz:

type View interface {

	// Init initiates the view.
	Init(pack *skyobject.Pack, headers *pack.Headers, mux *sync.Mutex) error

	// Update updates the view.
	Update(pack *skyobject.Pack, headers *pack.Headers, mux *sync.Mutex) error

	// Get obtains information from the view.
	Get(id string, a ...interface{}) (interface{}, error)
}

Actualmente, todas las vistas compiladas se almacenan en memoria. Pero debido al incremento de la base de usuarios se volverá poco práctico. En próximas publicaciones las vistas se guardarán en un  [key-value store](https://en.wikipedia.org/wiki/Key-value_database) 

Para la versión 0.2, hay dos implementaciones de Vistas (View); una por contenido (Tablones/Hilos/posts/votos) y otra para compilar una lista de seguir/evitar para los usuarios..

## Una Nueva Experiencia de Usuario

En el momento de realizar este post, esta cerca de completarse. Aquí hay un video (en inglés) del trabajo que se está realizando:

!(https://youtu.be/Oue3WVkmGh4)

Para mantenerte al día del desarrollo de Skycoin BBS, mira este blo y entra en nuestra [comunidad de Skycoin BBS en Telegram](https://t.me/skycoinbbs)

