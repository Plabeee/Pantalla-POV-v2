# Fork de Pantalla-POV-v2 
# para modificaciones del Hardware y 
# adaptaciones del Software

## Animaciones con solo una barra led

## ==== Modificaciones ====

* Se le añade una batería de 800mAH al aspa para utilizar la carga en la demanda cuando se conecte via Bluetooth o WiFi a la vez que enciende todos los leds.

* El engranaje(Pololu N20) tiene una forma cuadrada en vez de cilindral el equivalente de las partes conseguidas localmente.

Estas primeras modificaciones reemplazan las impresiones 3D en la carpeta llamadas nuevo_[nombre]

## ========

Hace unos meses construimos una pantalla POV. Este proyecto consistía en hacer girar una barra led muy rápido y cambiar los colores de los leds en el momento exacto para crear imágenes. Sin embargo, faltaban muchas cosas por mejorar. Sobre todo la resolución, ya que la barra led solo tenía 70 leds y medía 50 centímetros. La estructura tampoco era demasiado robusta y el microcontrolador solo podía guardar 9 imágenes en su memoria. 

[![](https://markdown-videos.deta.dev/youtube/RXTmUNzOTNc)](https://youtu.be/RXTmUNzOTNc)

## Resumen del proyecto pantalla holográfica

Como ya sabes, uno de los defectos de la primera pantalla holográfica es que tenía muy poca densidad de leds. Como no hemos encontrado tiras led de mayor densidad, hemos diseñado nuestra propia PCB, la cual lleva leds de 2 milímetros. La PCB mide casi 50 centímetros y tiene 200 leds, o sea que hemos multiplicado por 3 la densidad de la versión anterior.   

Además esta vez, en lugar de usar un Arduino Nano como controlador hemos utilizado una ESP32, en la cual caben 90 imágenes. Gracias a ello además de imágenes esta nueva versión de la pantalla también puede mostrar animaciones.   

Respecto a la estructura, al igual que en la versión anterior, la hemos impreso con una impresora 3D. Sin embargo, el diseño es mucho mejor. Es más estable y no da la sensación de que se vaya a romper nada más poner la barra led a girar.   

Otra ventaja del nuevo diseño es que para alimentar los leds de la barra led, en vez utilizar un anillo colector para pasar los cables, hemos usado una fuente de alimentación inalámbrica. De esta forma, no hay cables entre el estator (la base) y el rotor (la barra led).   

El enlace a todos los archivos del proyecto (diseño, gerber de la PCB, código, etc) está en el siguiente apartado, «lista de materiales». 

## Esquema Eléctrico

 ![](Esquema%20el%C3%A9ctrico%20Est%C3%A1tor%20Pantalla%20POV%20V2.jpg)
 ![](Esquema%20el%C3%A9ctrico%20Rotor%20Pantalla%20POV%20V2.jpg)

Por un lado, en la base de la pantalla holográfica está el motor de corriente continua conectado al driver, y a su vez éste está alimentado directamente por la fuente de alimentación. Como la fuente de alimentación inalámbrica requiere 12 Voltios, hemos añadido un transformador de voltaje para pasar los 5V a 12V.  

En el rotor (la parte que gira) está el receptor de la fuente de alimentación inalámbrica con las que alimentamos la PCB. Esta PCB tiene 200 leds WS2812B, 200 condensadores, una ESP32 y un sensor de efecto Hall que hemos conectado con cables al pin digital 12. Lo hemos tenido que conectar con cables porque se nos olvidó incluirlo en el diseño de la PCB. 

## Código de la pantalla holográfica
### Pantalla holográfica con imágenes

El código es un poco complicado de entender si no tienes experiencia programando. Sin embargo, vamos a ver que es lo que tienes que hacer para poder cambiar las animaciones e imágenes que muestra la pantalla holográfica.   

Lo primero, cuando tengas una imagen ya elegida, es cambiar su tamaño a 200 x 200 píxeles. Además si quieres que la imagen no tenga fondo, tienes que dejarlo de color negro, no transparente.   

Una vez tienes tu imagen de 200 x 200, abre el programa de Python llamado «process_images» y en la línea 7 cambias el nombre que hay escrito por el nombre de tu imagen. En el código podrás ver que está indicado donde tienes que cambiarlo.   

A continuación lo ejecutas con el comando «python3 .\process_images.py» y automáticamente se crea un archivo de texto llamado «img_converted.txt». Este archivo contiene la matriz que representa tu imagen en la pantalla POV. El nombre de la variable es «frameX». Este nombre lo puedes cambiar y poner el que tu quieras. Nosotros a estas variables las llamamos por orden número, es decir, «frame0», «frame1», etc.  

En este punto ya has terminado con el código de Python. Ahora tienes que abrir el código de Arduino llamado «PantallaPOV_V2», que son dos ficheros, «PantallaPOV_V2.ino» e «imgs.h». En «imgs.h» es donde tienes que copiar la matriz de tu nueva imagen. Verás que ya hay 5 matrices, que son las 5 imágenes que nosotros hemos utilizado. Puedes borrarlas o dejarlas, lo que tu quieras.   

Ya para terminar tienes que ir a las líneas de código 138, 142, 154 y 158 del archivo «PantallaPOV_V2.ino», y donde pone «frame1», sustituirlo por el nombre de la variable de tu nueva imagen. Otra cosa que puedes modificar es el brillo de los leds. Para ello solo tienes que ir a la línea 19 de este mismo archivo y modificar el valor de la variable BRIGHTNESS.  

## Pantalla holográfica con animaciones  

Si lo que quieres mostrar en la pantalla POV es una animación, lo primero que debes hacer después de encontrar el vídeo que quieres mostrar es escalarlo a 200 x 200 píxeles, al igual que las imágenes. Además ten en cuenta que en la ESP32 solo caben unas 80 – 90 imágenes. Por lo que si el vídeo está a 30 FPS, solo podrás mostrar 3 segundos, pero si está a 10 FPS, podrás mostrar 8 segundos, que es lo que hemos hecho nosotros.  

Una vez tienes tu vídeo escalado y recortado, tienes que abrir el código de Python «process_animations». En la línea 7 tienes que cambiar el nombre del vídeo que hay escrito y poner el nombre de tu vídeo. Cuando ejecutes este código se creará un archivo de texto llamado «video_converted» el cual guarda las matrices de todas las imágenes del vídeo.  

Ahora abres el código de Arduino llamado «PANTALLA_POV_V2_ANIMACIONES» y en el archivo llamado «imgs.h» copias las matrices.   

Ten cuidado de no borrar la variable que se encuentra al final llamada «frames». Sin embargo, si tienes que modificarla. Donde pone un «80» tienes que poner el número de tus imágenes. Y entre los corchetes, donde pone «frame0», «frame1», «frame2», etc., solo tienes que dejar hasta el número de imágenes que tengas.   

Por último, en el archivo principal de Arduino cambiar el valor de la variable «NUM_IMAGES» por el número de imágenes que tenga tu animación. Con esto ya estaría todo.  

Fuente: [@Wexter Home](https://www.wexterhome.com/proyectos-con-arduino/pantalla-holografica/)
