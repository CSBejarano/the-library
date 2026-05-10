# Agéntica Fichas

## Card 1

**Q:** ¿Qué es la "Habilidad Meta" de la Biblioteca (The Library Meta-Skill)?

**A:** Un método para coordinar y distribuir habilidades, agentes y prompts privados a través de múltiples bases de código y dispositivos.

---

## Card 2

**Q:** ¿Para qué perfil de ingeniero está diseñada específicamente la solución de la Biblioteca?

**A:** Ingenieros que gestionan más de 10 bases de código y desarrollan herramientas agénticas privadas y especializadas.

---

## Card 3

**Q:** ¿Cuál es el problema principal de copiar manualmente habilidades entre repositorios de agentes?

**A:** Las herramientas se desincronizan, se duplican y se vuelven difíciles de coordinar entre los miembros del equipo.

---

## Card 4

**Q:** En la ingeniería agéntica, ¿qué representan las "habilidades" (skills)?

**A:** Las capacidades brutas o funciones básicas que un agente puede ejecutar.

---

## Card 5

**Q:** ¿Qué función cumplen los "agentes" dentro del marco de trabajo propuesto por IndyDevDan?

**A:** Permiten alcanzar escala y paralelismo en la ejecución de tareas.

---

## Card 6

**Q:** ¿Cuál es el rol de los "prompts" en relación con las habilidades y los agentes?

**A:** Sirven para orquestar las capacidades de las habilidades y los agentes de niveles inferiores.

---

## Card 7

**Q:** El archivo de la biblioteca funciona como un punto de referencia único, similar a los archivos _____ o pyproject.toml.

**A:** package.json

---

## Card 8

**Q:** ¿Qué almacena el archivo YAML de la biblioteca en lugar del código fuente real?

**A:** Referencias o punteros a repositorios de GitHub existentes o rutas de archivos locales.

---

## Card 9

**Q:** ¿Por qué el autor enfatiza el uso de repositorios privados para almacenar habilidades agénticas?

**A:** Para proteger la propiedad intelectual y el valor comercial de soluciones especializadas por las que se recibe un pago.

---

## Card 10

**Q:** ¿Qué significa que la biblioteca sea una solución "primero para agentes" (agent-first)?

**A:** Es una habilidad que no requiere código asociado y puede ser operada completamente por un agente para autogestionarse.

---

## Card 11

**Q:** Menciona los cuatro pasos del flujo de trabajo completo para gestionar agentics.

**A:** Construir (Build), Catalogar (Catalog), Distribuir (Distribute) y Usar (Use).

---

## Card 12

**Q:** ¿Cuál es la función del comando `library add`?

**A:** Actualizar el catálogo de la biblioteca añadiendo punteros a nuevas habilidades, agentes o prompts.

---

## Card 13

**Q:** ¿Qué acción realiza el comando `library use`?

**A:** Instala local o globalmente las herramientas referenciadas en el catálogo para que el agente las utilice.

---

## Card 14

**Q:** ¿Para qué sirve el comando `library push` en un dispositivo de agente?

**A:** Para enviar las actualizaciones realizadas localmente en una habilidad de vuelta a su repositorio de origen.

---

## Card 15

**Q:** ¿Qué permite el comando `library sync` dentro de este sistema?

**A:** Asegurar que todas las bases de código y dispositivos tengan la versión más reciente del catálogo y del código.

---

## Card 16

**Q:** ¿Qué define a una "aplicación agéntica pura" (pure agentic application)?

**A:** Una solución que reside enteramente en prompts y habilidades, sin código de aplicación tradicional asociado.

---

## Card 17

**Q:** ¿Cuál es la ventaja de utilizar un "agente orquestador" en sistemas a gran escala?

**A:** Permite liderar y coordinar equipos de agentes y sistemas completos en dominios específicos.

---

## Card 18

**Q:** ¿Qué es un "cookbook" en el contexto de una habilidad de la biblioteca?

**A:** Una descripción en lenguaje natural de flujos de trabajo agénticos que indica cuándo y cómo usar cada comando.

---

## Card 19

**Q:** ¿Por qué es importante tener una "fuente única de verdad" al trabajar con múltiples dispositivos agénticos?

**A:** Para garantizar que todos los agentes y humanos operen con las mismas versiones de prompts y herramientas.

---

## Card 20

**Q:** En el sistema de Dan, ¿qué herramienta suele colocarse por encima de los prompts, agentes y habilidades para la automatización?

**A:** Un archivo `justfile`.

---

## Card 21

**Q:** ¿Qué diferencia hay entre una instalación de habilidad "local" y una "global"?

**A:** La local se limita al directorio del proyecto actual, mientras que la global es accesible desde cualquier parte del sistema.

---

## Card 22

**Q:** ¿Cómo ayuda la biblioteca a mejorar la "confianza" (trust) en los agentes?

**A:** Permite al desarrollador conocer exactamente qué líneas de código y prompts está ejecutando el agente en cada momento.

---

## Card 23

**Q:** ¿Qué permite el uso de comodines (como `meta-*`) en los comandos de la biblioteca?

**A:** Facilita la gestión masiva de múltiples elementos que comparten un prefijo común en su nombre.

---

## Card 24

**Q:** ¿Cuál es el propósito de integrar una "Mac Mini" como dispositivo de agente?

**A:** Actuar como un entorno de ejecución persistente y dedicado para que los agentes operen de forma independiente.

---

## Card 25

**Q:** El autor critica a los _____ que asumen que todo el código agéntico debe ser público.

**A:** vibe coders

---

## Card 26

**Q:** ¿Qué sucede con el catálogo de la biblioteca antes de que `library add` realice cualquier cambio?

**A:** El sistema realiza un `pull` para asegurarse de tener la última versión del catálogo remoto.

---

## Card 27

**Q:** ¿Qué utilidad tiene el archivo `skills.md` dentro de una habilidad agéntica?

**A:** Define la lógica, los comandos y la documentación que el agente utiliza para ejecutar la habilidad.

---

## Card 28

**Q:** Según el autor, ¿cuál es la tendencia principal en ingeniería agéntica para el año 2026?

**A:** El aumento de la confianza en los sistemas agénticos mediante el control total sobre sus herramientas y prompts.

---

## Card 29

**Q:** ¿Qué herramienta menciona el autor como alternativa personalizable a Claude Code para gestionar agentes?

**A:** PI coding agent (IPI).

---

## Card 30

**Q:** ¿Qué funcionalidad ofrece el comando `library list`?

**A:** Muestra todas las referencias actuales almacenadas en el archivo de la biblioteca y verifica su sincronización.

---

## Card 31

**Q:** Concepto: Agentic Primitives.

**A:** Los tres elementos fundamentales para construir sistemas de agentes: habilidades, agentes y prompts.

---

## Card 32

**Q:** ¿Cómo se gestiona el acceso de los agentes a los repositorios privados en este sistema?

**A:** Mediante el acceso a Git del sistema operativo donde corre el agente y las credenciales del equipo.

---

## Card 33

**Q:** ¿Qué permite la distribución agnóstica de la biblioteca?

**A:** Que las mismas capacidades estén disponibles en máquinas locales, sandboxes en la nube o dispositivos de compañeros.

---

## Card 34

**Q:** ¿Por qué el autor sugiere que un ingeniero de 1 o 2 repositorios no necesita la biblioteca?

**A:** Porque a esa escala es sencillo mantener los prompts y habilidades dentro del mismo repositorio sin duplicidad.

---

## Card 35

**Q:** ¿Cuál es el formato del archivo donde se definen las referencias de la biblioteca?

**A:** YAML.

---

## Card 36

**Q:** ¿Qué componente del sistema es responsable de crear otros prompts de manera estructurada?

**A:** La habilidad "meta-prompt".

---

## Card 37

**Q:** ¿Qué información proporciona el "argument hint" en un comando agéntico?

**A:** Ayuda visual o contextual sobre los parámetros que el comando espera recibir.

---

## Card 38

**Q:** ¿Cómo se evita que un agente realice acciones destructivas como `rm -rf` durante la instalación de habilidades?

**A:** Mediante sistemas de "control de daños" (damage control) integrados en el arnés del agente (como en PI coding agent).

---

## Card 39

**Q:** En lugar de un solo agente que lo haga todo, el autor recomienda un _____ de agentes especializados.

**A:** equipo (team)

---

## Card 40

**Q:** ¿Qué comando usarías para encontrar una habilidad específica si no recuerdas su nombre exacto?

**A:** library search

---

## Card 41

**Q:** ¿Qué sucede cuando se ejecuta `library use` con una ruta de GitHub?

**A:** El sistema clona el repositorio privado y mueve los archivos necesarios al directorio de habilidades del agente.

---

## Card 42

**Q:** ¿Cuál es el beneficio de separar las "raw capabilities" (habilidades) de la orquestación (prompts)?

**A:** Evita sobrecargar las habilidades con lógica excesiva, permitiendo que sean componentes reutilizables y simples.

---

## Card 43

**Q:** El autor afirma que el valor real no es un agente libre en el dispositivo, sino uno que opere bien contra _____.

**A:** problemas de dominio específicos.

---

## Card 44

**Q:** ¿Qué directorio se utiliza comúnmente en Claude Code para almacenar comandos (prompts)?

**A:** .claude/commands

---

## Card 45

**Q:** ¿Cómo se sincronizan las herramientas entre diferentes desarrolladores de un mismo equipo?

**A:** Compartiendo y sincronizando el mismo repositorio git que contiene el archivo `library.yaml`.

---

## Card 46

**Q:** La biblioteca permite referenciar tres tipos de ubicaciones: repositorios públicos, repositorios privados y _____.

**A:** rutas de archivos locales.

---

## Card 47

**Q:** ¿Qué importancia tiene el proceso de "catalogar" en la mentalidad de ingeniería de Dan?

**A:** Permite registrar herramientas sin interrumpir el flujo de desarrollo en el repositorio que genera valor.

---

## Card 48

**Q:** ¿Qué comando del sistema de la biblioteca permite ver la ayuda o manual de uso de cada función?

**A:** El comando de la habilidad misma (ej. `/library`) seguido del nombre del comando.

---

## Card 49

**Q:** ¿Cuál es el propósito final de escalar de agentes base a agentes orquestadores?

**A:** Lograr que la IA pueda operar sistemas y dispositivos completos de manera autónoma y coordinada.

---

## Card 50

**Q:** Verdadero o Falso: La biblioteca es una herramienta comercial que IndyDevDan está vendiendo.

**A:** Falso (es una idea conceptual y un repositorio de plantilla gratuito para que otros construyan el suyo).

---
