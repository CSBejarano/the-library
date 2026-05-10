# Agentes Cuestionario

## Question 1
¿Cuál es el problema principal que el 'Library meta-skill' busca resolver para los ingenieros que trabajan con múltiples agentes?

- [ ] La falta de potencia de procesamiento en dispositivos locales como el Mac Mini.
- [x] La duplicación y desincronización de habilidades, agentes y prompts en múltiples repositorios.
- [ ] La imposibilidad de utilizar modelos de lenguaje de código abierto en entornos privados.
- [ ] El alto costo de las suscripciones de API para agentes de orquestación.

**Hint:** Considera qué sucede cuando copias y pegas el mismo código en diez proyectos diferentes a lo largo del tiempo.

## Question 2
En el marco de trabajo de la 'Library', ¿qué función cumplen los 'Prompts' en relación con los 'Skills' y los 'Agents'?

- [x] Actúan como la capa de orquestación para los niveles inferiores.
- [ ] Proporcionan la capacidad bruta de ejecución de código.
- [ ] Permiten alcanzar escala y paralelismo en las tareas.
- [ ] Sirven como bases de datos vectoriales para el almacenamiento a largo plazo.

**Hint:** Es la capa superior que decide cuándo y cómo usar las herramientas disponibles.

## Question 3
¿Qué contiene realmente el archivo $library.yaml$ en este sistema de gestión?

- [ ] El código fuente completo de todas las habilidades en formato Base64.
- [ ] Un historial de todas las conversaciones pasadas con los agentes.
- [x] Referencias a repositorios de GitHub privados o rutas de archivos locales.
- [ ] Las credenciales y claves secretas para acceder a los servicios de nube.

**Hint:** Piensa en cómo funciona un gestor de paquetes que apunta a fuentes externas en lugar de incluir todo el software.

## Question 4
¿Cuál es la función específica del comando 'library use' dentro del flujo de trabajo descrito?

- [ ] Sincronizar el catálogo local con los cambios realizados por otros miembros del equipo.
- [ ] Añadir una nueva referencia de repositorio al archivo central de la biblioteca.
- [x] Instalar o clonar los elementos referenciados en el catálogo al entorno local o global.
- [ ] Ejecutar una prueba de diagnóstico en todas las habilidades instaladas.

**Hint:** Se trata del paso en el que traes las herramientas desde la 'nube' de repositorios a tu dispositivo actual.

## Question 5
¿Qué significa que la Library sea una solución 'agent-first' según el autor?

- [ ] Que solo puede ser operada por una IA y no por un ser humano.
- [x] Que el sistema es un 'skill' en sí mismo sin código asociado externo necesario para su invocación básica.
- [ ] Que fue programada íntegramente por un agente sin intervención humana.
- [ ] Que prioriza la velocidad de respuesta del agente sobre la seguridad del código.

**Hint:** Tiene que ver con la forma en que el sistema se presenta ante el agente, casi como una extensión de su propia naturaleza.

## Question 6
Si un desarrollador mejora un 'meta-prompt' localmente, ¿cómo se asegura de que esa mejora esté disponible para todos sus otros dispositivos y agentes?

- [ ] Debe enviar un correo electrónico con el nuevo archivo a su equipo.
- [ ] El sistema detecta automáticamente los cambios y los sube cada 5 minutos.
- [x] Utilizando el comando 'library push' para actualizar el repositorio de origen.
- [ ] Reiniciando el 'PI coding agent' para que reconstruya la biblioteca.

**Hint:** Es un término común en sistemas de control de versiones para enviar cambios locales a un servidor remoto.

## Question 7
¿Para qué tipo de usuario afirma Dan que este sistema NO es necesario?

- [ ] Ingenieros que trabajan en 10 o más bases de código simultáneamente.
- [x] Desarrolladores que operan exclusivamente en uno o dos repositorios.
- [ ] Equipos que necesitan compartir habilidades privadas de manera segura.
- [ ] Usuarios que emplean agentes de orquestación complejos.

**Hint:** Considera la escala del trabajo; si todo está en un solo sitio, ¿necesitas una herramienta de distribución externa?

## Question 8
Según la jerarquía de 'Agentic Engineering' mencionada, ¿cuál es el beneficio de separar 'Skills' de 'Agents'?

- [ ] Los 'Skills' permiten a los agentes interactuar con el mundo físico.
- [x] Evita sobrecargar las habilidades haciendo que realicen tareas de orquestación que corresponden a otros niveles.
- [ ] Reduce la latencia de las llamadas a la API del modelo de lenguaje.
- [ ] Permite que las habilidades se escriban en Python y los agentes en TypeScript.

**Hint:** Se trata de no intentar que una herramienta básica (raw capability) haga el trabajo de un líder o de un coordinador.
