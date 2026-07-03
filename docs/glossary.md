# Glosario

Vocabulario compartido del dominio y del proyecto. Mantén las definiciones cortas
y sin ambigüedad para que todo el equipo use los términos de la misma forma.

| Término   | Definición                                                                                          |
| --------- | --------------------------------------------------------------------------------------------------- |
| Categoría | Etiqueta de texto (`category`) que agrupa o clasifica una tarea; se almacena en el modelo `Task`.   |
| Tarea (Task) | Unidad de trabajo que crea un usuario. Entidad principal del dominio (modelo `Task`); tiene título, descripción, categoría y fecha de creación, y pertenece a un único usuario. |
| Usuario   | Persona autenticada en el sistema, representada por el modelo `User` estándar de Django. Cada usuario solo accede a sus propias tareas. |

> Convención: ordena los términos alfabéticamente y enlaza al documento donde se
> detalla cada concepto cuando aplique.
