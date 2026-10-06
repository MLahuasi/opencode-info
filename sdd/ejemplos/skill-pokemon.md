# Ejemplo: crear una Skill con OpenCode

> **Ejemplo de interacción con OpenCode**
>
> Este documento conserva un ejemplo del laboratorio OpenCode Pacman. Muestra una posible conversación y una respuesta generada; no es una Skill obligatoria ni una regla general para otros proyectos.

Como ejemplo se crea una Skill para consultar información de Pokémon utilizando la API pública de PokéAPI.

En modo `Plan` solicitar:

```text
Crea un skill que sirva para obtener la información de un pokemon cuando lo solicite.
Usa esta habilidad para consultar temas relacionados a Pokemon.
Para obtener la información de cada Pokemon consulta esta API: https://pokeapi.co/api/v2/pokemon
La API recibe ID o nombre del Pokemon como parámetro, por ejemplo:
https://pokeapi.co/api/v2/pokemon/1
https://pokeapi.co/api/v2/pokemon/ditto

Cualquier tema de conversación relacionado a Pokemon debe ser consultado en esta API.
```

OpenCode analizará el requerimiento y generará un plan.

Una vez aprobado el plan, cambiar a modo `Build` y solicitar su implementación.

La Skill se almacena utilizando una estructura similar a:

```text
.opencode/skills/<name>/SKILL.md
```

Para este laboratorio puede consultarse:

[`.agents/skills/pokemon-info/SKILL.md`](https://github.com/MLahuasi/opencode-pacman/tree/main/.agents/skills/pokemon-info/SKILL.md)

## Verificar la Skill

Después de crearla:

1. Reiniciar `OpenCode`.
2. Restaurar la sesión anterior.
3. Realizar una consulta que pueda utilizar la Skill.

Para restaurar la sesión:

```bash
# Continúa la última sesión de OpenCode.
opencode -c
```

En modo `Build`, por ejemplo:

```text
Quiero crear una tabla con la información del pokemon 4
```

OpenCode detectará que existe una Skill relacionada con Pokémon y podrá cargarla para resolver la solicitud.

Resultado:

![Respuesta de la Skill de Pokémon](../../assets/07-skill-pokemon-response.png)

---

[← Volver a Specs, Skills y agentes](../03-specs-skills-y-agentes.md) | [Temario](../../09-opencode-spec-driven-development.md) | [Siguiente →](refinamiento-spec-fantasmas.md)
