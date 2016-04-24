---
layout: post
section-type: post
title: Codeigniter
category: PHP
tags: [ 'codeigniter', 'php', 'web' ]
---

Acababa de salir de mi curso, un año progamando en Android y habiendo tocado lo justo de PHP como para hacer Wordpress y dar gracias al señor.
Primer día en la empresa, ya los conocía, me habían ofrecido trabajar allí antes de acabar el curso, pero decliné la oferta y esperé,
un saludo, toma tú MAC **(¿¡MAC!?)**, colócalo y lo configuras y si quieres le instalas Windows **(¡SALVADO, WINDOWS! (Una polla.))**, aquí
trabajamos con Codeigniter, jQuery, ... **(¿¡CODE..QUÉ!?)**.

Mi primer día empezaba de puta madre, jamás había tocado un MAC (¿para qué iba a querer yo algo tan sumamente caro?), y no sabía que cojones
era Codeigniter, me dan acceso al FTP del servidor y al propio servidor para tener acceso a la base de datos, mi compañero abre Chrome
y busca Codeigniter (qué coño sería eso), llega a la web y se pone a leer la documentación. Ja, documentación, si algo me caracteriza es
mi impaciencia, abro el FTP y me bajo los archivos que me habían dicho que eran necesarios para modificar una vista y añadir una 
funcionalidad, llega la primera pregunta: "¿Es qué sabes Codeigniter?", sigo sin saber de que me estás hablando, pero no voy a leer.

Este fué mi gran primer día, no tarde mucho en comprender como iba Codeigniter y aprender jQuery, así que, por si tu no lo sabes, 
voy a compartir mis conocimientos, bastante básicos, acerca de este framework.

## Codeigniter

Codeigniter es un framework para constuir sitios dinámicos con PHP, está basado en el modelo vista-controlador (MVC).

Los controladores son el centro de todo, ahí ocurre la magia, en la vista se muestra y en el modelo se piden datos a la base de datos.
Instalar Codeigniter es bastante sencillo, tan solo tienes que bajartelo de su web: [https://www.codeigniter.com/](https://www.codeigniter.com/)
y copiarlo en tu servidor, hasta ahí todo fácil, ahora llega lo complicado, al menos que ya sepas algo acerca de todo esto, en cuyo caso,
supongo que será sencillo de comprender.

#### Manos a la obra

Como ya he dicho, en el controlador ocurre la magia, si te has descargado Codeigniter basta con que abras la carpeta, vayas a application
y a controllers, ahí tienes uno de ejemplo, yo de todas formas he colgado un proyecto de ejemplo en mi [GitHub](https://github.com/Deblugger/codeigniterexample)

```php
public function index(){
	$this->load->view('welcome_message');
}
```

Esta es la función index del controlador, por si no lo sabías la forma de acceder a una vista es un poco diferente, suponiendo
que estés corriendo todo esto bajo un localhost, la ruta sería asi: localhost/codeigniter/index.php/[CONTROLADOR]/[FUNCION]/[PARAMETROS]

Si pones solo el controlador se llama automáticamente a la función index, la cual, a su vez, carga la vista *welcome_message.php*
(el secreto está en que las vistas van en **application/views**).

```php
public function ejemplo(){
	$data['mensaje'] = 'Hola CodeIgniter';
	$this->load->view('ejemplo', $data);
}
```

Los controladores permiten pasar datos mediante un array a las vistas, a estos datos se acceden con $variable.

```php
<?=$mensaje;?>
```

Esta es la vista a la que llama la función ejemplo, si te fijas lo unico que contiene es un echo (increible, gracias a este maravilloso
sistema ya no tienes que liarte a escribir **echo** cada vez que quieras ver algo, basta con ponerlo así). Por último, $this hace
referencia al propio controlador, que tiene todos sus métodos y los que hereda. Si nos vamos a nuestro navegador y abrimos:
**localhost/codeigniter/index.php/ejemplos/ejemplo** se nos mostrará una vista cuya única información será ese 'Hola CodeIgniter'
que le hemos pasado a través del controlador.

```php
public function ejemplo_modelo(){
	$this->load->model('model');
	$this->model->select_query('*');
}
```

Como ya te he dicho, los ataques a la base de datos se hacen a través del modelo, que se carga con:
**$this->load->model('nombre_del_modelo)**, me apuesto un huevo a que eres capaz de saber donde meterlo sin que yo te lo diga,
exacto, application/models. Además con $this->(nombre_modelo)->(nombre_función) puedes acceder a las funciones que hay en el modelo.

Te preguntarás, y con toda la razón del mundo, que como coño le digo como acceder la base de datos, pues la respuesta es que en 
**application/config** hay un archivo que se llama **database** que te permite configurar el acceso a dicha base de datos, vamos 
a echar un vistado al modelo:

```php
class Model extends CI_Model {
    function __construct(){
        parent::__construct();
    }
    
    function select_query($campo){
        $res = $this->db->select($campo)
            ->from('YOUR_TABLE')
            ->where('YOUR', 'clauses')
            ->get();
        // Esta sentencia da como resultado ->   SELECT ($campo) FROM YOUR_TABLE WHERE YOUR=($clauses)
        return $res->result();
    }
}
```
Como puedes ver, Codeigniter, trabaja con $this->db para atacar la base de datos, con esto se hace el select, insert, update, delete, ...
todo, vaya, todo tiene que pasar por ahí.

Si quieres más información te animo a que vayas a la web de [Codeigniter](http://www.codeigniter.com) y ahondes más con la documentación.

