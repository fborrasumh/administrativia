# AdministrativIA

Sistema de aprendizaje de **Derecho Administrativo I** (asignatura 1189, Grado en Derecho, Universidad Miguel Hernández de Elche) construido a partir de su Guía Docente 2026-2027. Es una aplicación web de un solo fichero: funciona en el navegador, sin servidor, sin cuentas y, salvo el feedback de IA, sin conexión.

**Usar la app:** https://fborrasumh.github.io/administrativia/

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23891817.svg)](https://doi.org/10.5281/zenodo.23891817)

**Idiomas:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL). Las citas de la ley, el texto literal de la guía y los epígrafes se muestran siempre en su original castellano.

## Qué hace

- **Ruta adaptativa.** Las siete unidades de la guía (una por objetivo de aprendizaje) tienen un nivel de dominio que sube al acertar, baja al fallar y se desgasta con el tiempo. La siguiente actividad se elige con reglas fijas y reproducibles: primero una pregunta de diagnóstico por unidad, después lo fallado que toca repasar y la unidad más floja, sin repetir la misma unidad tres veces seguidas. La app muestra siempre el motivo.
- **Práctica de recuperación.** 93 preguntas de opción única y 21 preguntas cortas «en positivo» (el formato alternativo de examen que prevé la guía), en español, inglés y portugués. Cada explicación cita un fragmento literal del artículo que la sostiene, con enlace para leerlo entero.
- **Casos resueltos por reglas, no por IA.** Siete generadores: vía administrativa y recurso procedente, validez y vía de revisión de oficio, cómputo de plazos (art. 30 de la Ley 39/2015, con festivos y prórroga), sentido del silencio, técnicas de alteración de la competencia, reparto Estado-Comunidades Autónomas (las materias se leen del texto de los arts. 148.1 y 149.1 CE) y tipos de norma. Si una fecha coincide con un error típico (contar días naturales, contar el día de la notificación, no prorrogar), la app dice cuál.
- **Escritura con feedback.** Preguntas de desarrollo sobre los 17 temas de la guía (con sus epígrafes literales como guion) y comentario de sentencias que pega el estudiante. La IA comenta, pero no pone nota; el código comprueba cada cita del texto del estudiante o de la sentencia, cada epígrafe y cada artículo que menciona la IA contra el texto del BOE incluido, retira cualquier calificación y marca con ⚠ lo que no se verifica.
- **Medidor de literalidad.** La guía no da por buenas las respuestas «excesivamente literales». La app calcula qué parte del texto repite secuencias de seis palabras de la ley o de los apuntes que el estudiante pegue (estos no salen del navegador). Es orientativo: la guía no fija un umbral.
- **Simulacro** con los dos formatos de la guía: dos preguntas de desarrollo, con la regla de un mínimo de 2,5 sobre 5 en cada una, o cinco preguntas cortas de 2 puntos. Sin límite de tiempo.
- **Normativa sin conexión.** 443 artículos del texto consolidado del BOE: Constitución, Leyes 39/2015 y 40/2015, Ley 7/1985, Ley 50/1997 y Ley 29/1998, con buscador.
- **Panel del profesorado.**
  - Asignación de unidades a la primera y segunda parte del examen (la guía no la concreta).
  - Validación pregunta a pregunta, con exportación a un fichero de configuración.
  - Analítica de ítems a partir de los ficheros de progreso que el alumnado comparte voluntariamente: dificultad, discriminación biserial puntual y distractores que nadie elige. Las propuestas de cambio las acepta o descarta el profesorado.
  - Exportación del banco a Moodle XML y GIFT.

## Por qué así

Se apoya en mecanismos con evidencia sólida: práctica de recuperación (Roediger y Karpicke, 2006, doi:10.1111/j.1467-9280.2006.01693.x), práctica espaciada (Cepeda et al., 2006, doi:10.1037/0033-2909.132.3.354), práctica intercalada (Rohrer y Taylor, 2007, doi:10.1007/s11251-007-9015-8) y feedback inmediato elaborado. No usa «estilos de aprendizaje».

## Cómo se usa la IA

Solo para el feedback de escritura, con la clave propia de OpenAI, Google Gemini o Anthropic Claude. La clave se guarda solo en el navegador y se envía únicamente al proveedor elegido. Todo lo demás funciona sin clave. El botón «Ver un ejemplo» muestra un feedback precalculado que pasa por las mismas comprobaciones.

## Privacidad

En el navegador (IndexedDB) se guardan las respuestas, el progreso, los textos y la configuración del profesorado. Hacia el proveedor de IA sale solo, cuando el estudiante lo pide, su respuesta de desarrollo (y la sentencia, si la comenta), con correos, teléfonos y DNI enmascarados. Antes del primer envío se muestra exactamente lo que sale y hay que confirmarlo. El fichero de progreso para el profesorado lleva un seudónimo elegido por el estudiante, el dominio por unidad y la opción marcada en cada pregunta; no lleva textos ni nombre.

## Límites

- **Los contenidos de la versión 1.0 están pendientes de validación jurídica por el profesorado.** La app lo indica en portada y lleva el recuento de preguntas validadas. Las citas de la ley sí están comprobadas por código: las 175 son literales.
- La normativa es informativa, consolidada a las fechas que indica la app; la versión oficial es la del BOE. Si una norma cambia, hay que regenerar `leyes.js` (scripts en `tools/`).
- No incluye todavía la normativa propia de la Comunitat Valenciana (Administración de la Generalitat) ni el proceso contencioso-administrativo en detalle; el plazo contencioso frente a actos presuntos no se evalúa.
- Los casos usan supuestos simplificados: los festivos los fija el enunciado; en la vía administrativa se asume que la norma sectorial no dice otra cosa.
- La detección de ideas en preguntas cortas busca palabras clave: es una pista, no una corrección. La puntuación del simulacro es autoevaluación.
- El feedback de IA puede equivocarse aunque sus citas estén verificadas: una cita literal no garantiza que el comentario sea correcto.
- La app no sustituye a las clases, al manual ni al examen.

## Pruebas

- `node tools/check_citas.js`: las 175 citas del currículo aparecen literalmente en el texto del BOE incluido.
- `node tools/test_motor.js`: 6.036 comprobaciones de lógica pura. Cubre plazos con verdad conocida (fin de mes, año bisiesto, prórroga, festivos, agosto hábil), determinismo de los siete generadores en 2.000 casos cada uno, reglas de vía y recurso, intercalado, repaso, literalidad y analítica de ítems.
- `python3 tests/prueba_completa.py`: 59 comprobaciones en Chromium con IA simulada (OpenAI, Gemini y Claude). Incluye:
  - citas, epígrafes y artículos inventados detectados, nota retirada, JSON roto, error 429 y enmascarado de datos;
  - persistencia, Word sin red, CSV, Moodle XML, GIFT y copia de seguridad;
  - tres idiomas;
  - **axe-core: cero infracciones WCAG 2.2 AA en 10 pantallas × 3 idiomas**;
  - uso sin conexión, móvil a 390 px sin desborde y gama baja (CPU ×4: primera pantalla útil en menos de 1 s).

## Adaptarla a otra universidad

El currículo es un fichero de datos (`src/curriculum.js`): unidades, ítems, temas y casos. La lógica (`src/motor.js`) no depende de la asignatura. Se monta con `python3 build.py`.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche, ORCID 0000-0002-5519-4573) y José Antonio Tardío Pato (Universidad Miguel Hernández de Elche).

A partir de la Guía Docente 2026-2027 de la asignatura 1189 Derecho Administrativo I (Grado en Derecho, UMH); contenidos jurídicos sujetos a la validación del profesor José Antonio Tardío Pato.

Texto normativo basado en datos de la Agencia Estatal Boletín Oficial del Estado (https://www.boe.es), obtenidos del repositorio abierto legalize-es. Las disposiciones legales no están sujetas a propiedad intelectual (art. 13 del Texto Refundido de la Ley de Propiedad Intelectual).

## Licencia

MIT.
