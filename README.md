1. Creamos la carpeta que va a contener todos los archivos y los vamos completando con el codigo correspondiente.

2. A continuación con la terminal de Laragon accedemos a la carpeta del proyecto y creamos un repositorio en Git para poder salvar el proyecto segun avancemos.

3. Con la misma terminal accedemos a mysql y creaamos un usario y la contraseña y una base de datos

4. Cambiamos la zona de trabajo a esa base de datos y mediante los siguientes codigos en la terminal creamos las tablas de usuarios y noticias y las contraseña encriptadas para cada usuario:

mysql> CREATE TABLE `usuarios` (   `id` int(3) NOT NULL AUTO_INCREMENT,   `usuario` varchar(16) NOT NULL,   `clave` varchar(64) NOT NULL,   `fecha_acceso` datetime DEFAULT NULL,   `activo` tinyint(1) NOT NULL DEFAULT '0',   `usuarios` tinyint(1) NOT NULL DEFAULT '0',   `noticias` tinyint(1) NOT NULL DEFAULT '1',   PRIMARY KEY (`id`),
  UNIQUE KEY `usuario` (`usuario`),   UNIQUE KEY `id` (`id`) ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.41 sec)

mysql> CREATE TABLE `noticias` (   `id` int(3) NOT NULL AUTO_INCREMENT,   `titulo` varchar(32) NOT NULL DEFAULT '',   `slug` varchar(36) DEFAULT '',   `entradilla` varchar(128) DEFAULT '',   `texto` longtext,   `activo` tinyint(1) NOT NULL DEFAULT '0',   `home` tinyint(1) NOT NULL DEFAULT '0',   `fecha` datetime DEFAULT NULL,   `autor` varchar(64) DEFAULT NULL,   `imagen` varchar(64) DEFAULT NULL,   PRIMARY KEY (`id`),   UNIQUE KEY `id` (`id`) ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.57 sec)


mysql> INSERT INTO usuarios VALUES     (1,"admin","$2y$12$0ADfEn5VlH2MdoOwBNmdT.opTLEMbRErYmtKB0AIigpeXOMCeNeM.?>",null,1,1,1),     (2,"operador1","$2y$12$TcQwsNMf5paSD2IVpKNk9e48H5eY6/nuchiC7/MabsV2MxWgoViD.?>",null,1,0,1),     (3,"operador2","$2y$12$OxN1YezFvNvTUzZ82woY..1lXqze2pJlZ41pM70dvAy6kqLg/6vkm?>",null,0,0,1) ;

INSERT INTO noticias VALUES (1,
                             "Gryffindor",
                             "gryffindor",
                             "La Casa Gryffindor fue fundada por el célebre mago Godric Gryffindor.",
                             "Godric solo aceptaba en su casa a aquellos magos y brujas que tenían valentía, disposición y coraje, ya que estas son las cualidades de un auténtico Gryffindor. Los colores de esta casa son el dorado y el escarlata y su símbolo es un león. La reliquia más preciada de la casa es la espada de Godric Gryffindor, perteneciente, como su nombre indica, al fundador de la casa. Los estudiantes de esta casa pasan la mayor parte del tiempo en la Torre de Gryffindor, ubicada en el séptimo piso del Castillo de Hogwarts.",
                             1,
                             1,
                             "2019-07-22 00:00:00",
                             "https://harrypotter.fandom.com",
                             "gryffindor.jpg"),
                            (2,
                             "Hufflepuff",
                             "hufflepuff",
                             "La Casa Hufflepuff se encuentra en una bodega en el mismo pasillo subterráneo que en el de la cocina.",
                             "Hufflepuff anteriormente buscaba alumnos que quisieran pertenecer a esa casa de puro consentimiento, aunque actualmente busca alumnos leales, honestos, que no temen al trabajo pesado. La fundadora es nada menos que la bruja, amiga de toda la vida de Rowena Ravenclaw, Helga Hufflepuff. Helga, fue una bruja muy noble, amigable y la principal impulsora de que Hogwarts aceptase a alumnos nacidos de muggles. La principal reliquia de la casa es la copa de Helga Hufflepuff. El símbolo de la casa es un tejón negro y sus colores representativos son el amarillo y el negro carbón.",
                             1,
                             1,
                             "2019-07-23 00:00:00",
                             "https://harrypotter.fandom.com",
                             "hufflepuff.jpg"),
                            (3,
                             "Ravenclaw",
                             "ravenclaw",
                             "La Casa Ravenclaw se encuentra en una torre en el ala oeste del castillo.",
                             "Sus colores son el azul y el bronce. Ravenclaw busca alumnos académicos, estudiosos y que siempre sepan lo que hay que hacer. Fue fundada por la bruja, nacida en la Canada, Rowena Ravenclaw. Supuestamente la principal inventora del nombre, lugar y formato de Hogwarts. Ella misma es la causante de que las escaleras se muevan. Su principal reliquia es la diadema de Rowena Ravenclaw. El símbolo de la casa es el águila, aunque en alguna versión del escudo es un cuervo.",
                             1,
                             0,
                             "2019-07-24 00:00:00",
                             "https://harrypotter.fandom.com",
                             "ravenclaw.jpg"),
                            (4,
                             "Slytherin",
                             "slytherin",
                             "La Casa Slytherin se caracteriza principalmente por la ambición y la astucia.",
                             "Fue fundada por el mago Salazar Slytherin. La Sala Común de esta casa está situada en las mazmorras, pasando por un serie de numerosos pasillos subterráneos. Posiblemente se llega a ellos a través del Vestíbulo de Hogwarts . Específicamente se encuentra debajo del Lago Negro, haciendo que la sala común sea fría y con una tonalidad verdosa, ya que hay ventanas que dan a las aguas. Se accedea ella por una puerta altamente disimulada en un muro de piedra, diciendo una contraseña requerida. La única conocida es Sangre Pura. Su principal reliquia es el guardapelo de Salazar Slytherin. El animal representativo es la serpiente, sus colores son verde y plateado y el elemento es el agua asociada con la astucia y frialdad.",
                             0,
                             0,
                             "2019-07-25 00:00:00",
                             "https://harrypotter.fandom.com",
                             "slytherin.jpg");

5. Accedemos desde laragon a PhpMyAdmin para asegurarnos de garantizar todos los privilegios al usuarios que hemos creado y cercionarnos de que las tablas han sido creadas correctamente.

6. En el archivo DbHelper.php cambiamos los datos que vemos por nuestros propios datos como vemos en el siguiente ejemplo:

<?php

namespace App\Helper;

class DbHelper {
    
    var $db;
    
    function __construct(){
        
        //Conexión mediante PDO
        $opciones = [\PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8"];
        try {
            $this->db = new \PDO(
                'mysql:host=localhost;dbname=hogwarts2',
                'bd4',
                'concevigo',
            $opciones);
            $this->db->setAttribute(\PDO::ATTR_ERRMODE, \PDO::ERRMODE_EXCEPTION);
        } catch (\PDOException $e) {
            echo 'Falló la conexión: ' . $e->getMessage();
        }
        
    }
    
}

7. Una vez nos aseguremos de que todos los datos son correctos la pagina deberia ir con normalidad y permitirnos acceder a todos los apartados.






































# APP con Modelo Vista Controlador


## Pasos

1. En la terminal de Laragon creamos las carpetas del proyecto, atomizando cada una de las partes del mismo.
   
2. Creamos una Base de Datos con MySql mediante la terminal de Laragon.
   
3. Introducimos contenido en la BBDD mediante la terminal de Laragon:
   
   a) TABLA USUARIOS

    USE cms;
    CREATE TABLE `usuarios` (
    `id` int(3) NOT NULL AUTO_INCREMENT,
    `usuario` varchar(16) NOT NULL,
    `clave` varchar(64) NOT NULL,
    `fecha_acceso` datetime DEFAULT NULL,
    `activo` tinyint(1) NOT NULL DEFAULT '0',
    `usuarios` tinyint(1) NOT NULL DEFAULT '0',
    `noticias` tinyint(1) NOT NULL DEFAULT '1',
    PRIMARY KEY (`id`),
    UNIQUE KEY `usuario` (`usuario`),
    UNIQUE KEY `id` (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

    b) TABLA NOTICIAS
    
    USE cms;
    CREATE TABLE `noticias` (
    `id` int(3) NOT NULL AUTO_INCREMENT,
    `titulo` varchar(32) NOT NULL DEFAULT '',
    `slug` varchar(36) DEFAULT '',
    `entradilla` varchar(128) DEFAULT '',
    `texto` longtext,
    `activo` tinyint(1) NOT NULL DEFAULT '0',
    `home` tinyint(1) NOT NULL DEFAULT '0',
    `fecha` datetime DEFAULT NULL,
    `autor` varchar(64) DEFAULT NULL,
    `imagen` varchar(64) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `id` (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

4. Asimismo, introducimos contenido en la tabla de noticias usando la terminal de Laragon.

5. Creación de un archivo PHP para obtener el hash de las contraseñas.

6. Añadimos el contenido de los componentes del header y del footer.

7. Añadimos contenido al index.php dentro de view/app.

8. Añadimos contenido a acerca-de, noticia y noticias.
   
9. Añadimos una entrada extra a la sección de noticias utilizando el panel de administración:

![Panel admin](https://i.ibb.co/m9JWBQM/panel-admin.png)
![Noticias admin](https://i.ibb.co/HFf6q0Z/noticias-admin.png)


10. Creación de contacto.php para crear una nueva página dentro de la APP y modificación del archivo header.php para incluir la página en el menú de navegación.

11. En AppController.php creamos la función:
    
     public function contacto(){
        //Llamo a la vista
        $this->view->vista("app", "contacto");
    }

12. En el index.php contenido en public añadimos lo siguiente:
    
    switch ($ruta){

    //Front-end
    case "":
    case "/":
        controller()->index();
        break;
    case "acerca-de":
        controller()->acercade();
        break;
    case "contacto":
        controller()->contacto();
        break;
    case "noticias":
        controller()->noticias();
        break;
    case (strpos($ruta,"noticia/") === 0):
        controller()->noticia(str_replace("noticia/","",$ruta));
        break;

para crear el caso que se ejecuta cuando la persona usuaria clicka en el apartado Contacto del Header.

13. Finalmente, ejecutamos el ejercicio con Laragon y ¡ya tenemos nuestra página con Frontend y Backend!

## Capturas del resultado final:

![Home](https://i.ibb.co/dr3gbHC/Fire-Shot-Capture-023-Noticias-de-Harry-Potter-ejercicio-mvc-test.png)
![Noticias](https://i.ibb.co/Js0YT5j/MVC-HP-02.png)
![Noticia1](https://i.ibb.co/g70v5zQ/gryffindor.png)
![Noticia2](https://i.ibb.co/Mp7q3vb/snitch.png)
![About](https://i.ibb.co/rmTbmcW/MVC-HP-03.png)
![Admin](https://i.ibb.co/8N9qnMt/MVC-HP-04.png)


    
