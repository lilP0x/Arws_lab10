### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW

### HECHO POR:

JUAN PABLO FERNANDEZ
CESAR DAVID AMAYA

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/es-es/free/students/). Al hacerlo usted contará con $100 USD para gastar durante 12 meses.
Antes de iniciar con el laboratorio, revise la siguiente documentación sobre las [Azure Functions](https://www.c-sharpcorner.com/article/an-overview-of-azure-functions/)

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Escalabilidad Serverless (Functions)

1. Cree una Function App tal cual como se muestra en las  imagenes.

![](images/part3/part3-function-config.png)

![](images/part3/part3-function-configii.png)

Esta es la pantalla de confirmacion despues de haber creado la funcion.
![](/images/lab/confirmation_creation.jpeg)



2. Instale la extensión de **Azure Functions** para Visual Studio Code.

![](images/part3/part3-install-extension.png)

Posteriormente tendremos que instalar dicha extension en visual studio code.

![](/images/lab/image.png)

3. Despliegue la Function de Fibonacci a Azure usando Visual Studio Code. La primera vez que lo haga se le va a pedir autenticarse, siga las instrucciones.

![](images/part3/part3-deploy-function-1.png)

![](images/part3/part3-deploy-function-2.png)

4. Dirijase al portal de Azure y pruebe la function.

antes de esto, surigio un error que tuvimos que solucionar modificando el archivo host.json, para que la funcion pudiera ser desplegada

![](/images/lab/arreglando_error.png)

Despues de los pasos que anteriormente fueron mencionados entonces tendremos que probar la funcion para eso tendremos que ir a la seccion de test y mandar el siguiente comando.

{
    nth:xxxx
}

reemplazando las x con el numero que se requiere de la secuencia de Fibonacci.

![](/images/lab/antes_del_test.png)

[](/images/lab/test.jpeg)


Aqui podemos ver como la peticion a la funcion respondio con un mensaje 200 eso quiere decir OK por lo que la funcion esta funcionando adecuadamente.

![](images/part3/part3-test-function.png)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.

Ahora tendremos que hacer uso de postman con newn y para eso modificaremos los archivos del laboratorio pasado.

primero tendremos que modificar el codigo para mandar la peticion post al link que corresponde a la funcion

![](/images/lab/modificando-codigo.png)

tambien tendremos que modificar la coleccion como mostramos en la siguiente imagen.

![](modificando_coleccion.png)


6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

**Preguntas**

* ¿Qué es un Azure Function?
    Azure Functions es un servicio en la nube ofrecido por Microsoft Azure para ejecutar pequeñas piezas de código, conocidas como functions, en respuesta a eventos o solicitudes. Estas funciones son unidades independientes de lógica que pueden ser activadas por desencadenadores como solicitudes HTTP, mensajes en una cola o eventos en una base de datos.

    Azure Functions se enmarca dentro del modelo serverless computing, donde el desarrollador no administra directamente los servidores, sino que se centra exclusivamente en escribir el código.

* ¿Qué es serverless?

    Serverless es un modelo de computación en la nube donde el proveedor gestiona la infraestructura, el escalado y la disponibilidad, mientras que los desarrolladores solo se enfocan en el código. Aunque el término implica "sin servidores", en realidad hay servidores involucrados, pero están completamente abstraídos para los usuarios.

* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App?

    El runtime de Azure Functions es el entorno de ejecución que ejecuta las funciones. Es una combinación de bibliotecas y configuraciones que interpretan y ejecutan el código en el lenguaje elegido (como .NET, JavaScript, Python, Java, etc.).


* ¿Por qué es necesario crear un Storage Account de la mano de un Function App?

Un Storage Account es esencial para el funcionamiento de Azure Functions porque:

- Gestión de datos temporales: Las funciones almacenan datos temporales, como colas o sesiones, en este almacenamiento.
- Escalado: Permite mantener el estado compartido entre instancias al escalar.
- Durabilidad: Almacena logs, configuraciones y datos relacionados con las ejecuciones.
- Requerimientos internos: Algunos servicios como AzureWebJobsStorage dependen directamente del almacenamiento.

* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.
    Azure Functions ofrece tres principales tipos de planes:

1. Consumo
Características:
- Escalado dinámico según la demanda.
- Facturación basada en tiempo de ejecución y memoria utilizada.
- Ventajas:
- Ideal para cargas de trabajo esporádicas.
- Bajo costo inicial.
- Desventajas:
- Tiempo máximo de ejecución: 5 minutos (puede ampliarse a 10 minutos).
- Latencia en arranques en frío.
2. Premium
Características:
- Instancias preconfiguradas para evitar arranques en frío.
- Compatible con redes virtuales.
Ventajas:
- Sin límites en el tiempo de ejecución.
- Escalado más rápido y personalizable.
Desventajas:
- Más costoso que el plan de consumo.
3. Dedicado (App Service Plan)
Características:
- Funciona en instancias reservadas del App Service Plan.
- Uso ilimitado del tiempo de ejecución.
Ventajas:
- Reutilización de infraestructura existente.
- Control completo sobre los recursos.
Desventajas:
- No escala dinámicamente.
- Costo constante, incluso si no hay actividad.

* ¿Por qué la memorization falla o no funciona de forma correcta?
    La memoization puede fallar debido a:

- Cambios en el estado mutable: Si los datos cambian pero el almacenamiento en caché no se actualiza.
- Errores en la clave: La memoization depende de claves únicas; claves incorrectas generan resultados inconsistentes.
- Limites de almacenamiento: Algunas implementaciones tienen restricciones en la cantidad de datos que se pueden almacenar.
- Funciones no puras: Si la función depende de datos externos no controlados, los resultados pueden variar.


* ¿Cómo funciona el sistema de facturación de las Function App?


Azure Functions utiliza un modelo de pago por uso, donde se factura según:

- Número de ejecuciones: Cada vez que se ejecuta una función.
- Duración de la ejecución: Tiempo en que la función está activa (redondeado al milisegundo).
- Memoria utilizada: Recursos consumidos durante la ejecución.


El plan de consumo incluye una asignación gratuita mensual (1 millón de ejecuciones y 400,000 GB-s).
