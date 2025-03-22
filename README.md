Informe Técnico Sistema inteligente de Judo
1. Introducción al Sistema y su Objetivo
El sistema experto ha sido diseñado para analizar información contenida en diversos archivos (TXT, CSV y PDF), almacenados de manera local en el drive y responder preguntas relacionadas con el tema del Judo, el Judo es un arte marcial de origen Japones en donde el objetivo es tratar de derribar al otro y que caiga con la espalda tocando el suelo u obligándolo a someterse utilizando diversas técnicas de suelo. Su objetivo es proporcionar a los usuarios respuestas precisas y contextualizadas basadas únicamente en el contenido extraído de los documentos, aprovechando las capacidades avanzadas de generación de lenguaje natural de la API de OpenAI. Esto permite que, a partir de una base documental, el asistente se comporte como un experto en Judo, facilitando el acceso a información relevante y específica del dominio.
2. Descripción de la Arquitectura Utilizada
La arquitectura del sistema se ha desarrollado en Python y se ejecuta en Google Colab, aprovechando las siguientes etapas y módulos:
•	Montaje de Google Drive:
Se monta Google Drive para acceder a los archivos necesarios, tanto la API key como los documentos de Judo, los cuales se encuentran en rutas específicas dentro del Drive. Esto asegura un almacenamiento seguro y centralizado.

•	Gestión Segura de Credenciales:
La API key de OpenAI se almacena en un archivo ubicado en Google Drive. Para evitar su exposición en el código fuente, se utiliza una variable de entorno (OPENAI_API_KEY). Si esta variable no está definida, se lee el archivo y se asigna su contenido a la variable, reduciendo el riesgo de filtración al no "quemarla" directamente en el código.

•	Procesamiento de Archivos:
El sistema es capaz de procesar archivos en múltiples formatos:
o	TXT: Se leen como archivos de texto plano.
o	CSV: Se utilizan librerías como pandas para convertir el contenido en un formato legible (cadena de texto).
o	PDF: Se emplea la librería PyPDF2 para extraer el contenido textual de cada página del documento.
Cada archivo es leído y se almacena su contenido junto con su nombre (utilizado como título) en una estructura de datos que posteriormente se combina en un único bloque de texto.

•	Construcción del Contexto:
Los contenidos extraídos de todos los documentos se concatenan en una sola variable denominada “contexto”. Este bloque de texto es fundamental, ya que se utiliza para construir el prompt que se enviará a la API, asegurando que el modelo disponga de toda la información disponible sobre Judo.

•	Integración con la API de OpenAI:
Se realiza una llamada a la API mediante el método ChatCompletion.create(), donde se configura:
o	El modelo: En el ejemplo se utiliza "gpt-4o mini" (este identificador puede ser modificado según el modelo disponible).
o	El mensaje del sistema: Se define el rol del asistente como experto en análisis de Judo, instruyendo al modelo para que se base únicamente en la información extraída de los documentos.
o	El prompt: Se construye combinando el contexto de los documentos y la pregunta del usuario, garantizando respuestas fundamentadas.
o	Parámetros adicionales: Se utilizan parámetros como temperature y max_tokens para controlar la creatividad y extensión de las respuestas.

•	Interfaz de Usuario:
La interacción se realiza a través de una interfaz de consola simple, en la que el usuario puede ingresar preguntas de forma iterativa. El sistema procesa cada entrada, consulta la API y muestra la respuesta correspondiente.
3. Explicación de la Integración con GPT
El proceso de integración con la API de OpenAI se resume en los siguientes pasos:
1.	Preparación del Prompt:
Se combina el contenido extraído de los documentos (contexto) con la pregunta ingresada por el usuario. Este prompt instruye al modelo para que responda únicamente basándose en la información disponible sobre Judo.

2.	Definición del Rol del Asistente:
A través del mensaje de sistema se especifica que el asistente es un experto en análisis de Judo. Esto orienta al modelo a generar respuestas técnicas y coherentes, utilizando el contexto proporcionado.


3.	Configuración y Llamada a la API:
Se llama a openai.ChatCompletion.create() con los parámetros configurados, incluyendo el modelo, el prompt y ajustes como la temperatura y el número máximo de tokens. La API procesa la solicitud y devuelve una respuesta estructurada.
4.	Procesamiento y Visualización de la Respuesta:
La respuesta generada se extrae del objeto retornado y se muestra en la consola para que el usuario pueda evaluarla en tiempo real.

4. Ejemplos de Pruebas Realizadas y Resultados Obtenidos

•	Prueba de Lectura y Procesamiento de Archivos:
o	Objetivo: Verificar que el sistema pueda leer y extraer correctamente el contenido de archivos en formato TXT, CSV y PDF.
o	Procedimiento: Se colocaron documentos de prueba en la carpeta designada en Google Drive y se ejecutó el código para la extracción de contenido.
o	Resultado: Los archivos se leyeron sin errores y el contenido se consolidó en la variable “contexto”, listo para ser utilizado en el prompt.

•	Prueba de Integración y Generación de Respuestas:
o	Objetivo: Evaluar la capacidad del sistema para responder preguntas específicas sobre Judo.
o	Procedimiento: Se ingresaron preguntas como “¿Cuáles son las técnicas básicas del Judo?” mediante la interfaz de consola.
o	Resultado: El sistema envió el prompt a la API de OpenAI y se obtuvieron respuestas precisas, mencionando técnicas reconocidas como "o-soto-gari" y "seoi-nage", demostrando la correcta integración entre la extracción de información y la generación de respuestas.

•	Prueba de Seguridad de la API Key:
o	Objetivo: Comprobar que la API key se gestione de manera segura y no se exponga en el código.
o	Procedimiento: Se verificó la existencia de la variable de entorno OPENAI_API_KEY y se realizó la carga condicional de la misma.
o	Resultado: La API key se asignó correctamente a la variable de entorno y se utilizó de forma segura, reduciendo el riesgo de exposición en el código compartido.

5. Conclusiones
El sistema experto para análisis de Judo ha demostrado una integración efectiva entre el procesamiento de archivos en múltiples formatos y la generación de respuestas mediante la API de OpenAI. Se busco la gestión segura de la API key mediante variables de entorno garantiza una mayor protección de las credenciales. Los resultados obtenidos en las pruebas evidencian la robustez y precisión del sistema, posicionándolo como una herramienta valiosa para la consulta de información especializada en Judo. Sin embargo, si analizamos que toda la información aunque se procese muy bien, no me parece recomendable que una empresa que tenga información muy sensible tenga que subir o “regalar” su data hacia estas empresas en la nube, sería bueno aprender a explorar alternativas en sitio o que cada empresa pueda gestionar estos tipos de soluciones de manera propia, por seguridad y privacidad de la información.

Realizado por:
phorspcuc
Introducción a la IA
22 Marzo 2025

