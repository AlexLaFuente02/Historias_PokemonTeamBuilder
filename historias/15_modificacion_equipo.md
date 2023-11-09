# Historia: Edición de Equipos Pokémon Existentes ✏️

- **Yo como**: Entrenador Pokémon 🧢
- **Quiero**: Poder editar los equipos Pokémon que ya he creado 🔄
- **Para**: Ajustar mi estrategia y mejorar mi rendimiento en las batallas 🏆.

## Pendientes de definición 📝

1. ¿Se permitirá cambiar el nombre del equipo durante la edición?
   R. Ezequiel considera que esto si puede ser agregado para el primer prototipo.

## Especificación de requerimientos 📄

1. Los usuarios deben ser capaces de modificar la composición de sus equipos, incluyendo añadir o quitar Pokémon 🛠️.
2. La interfaz de edición debe ser intuitiva, permitiendo realizar cambios de manera rápida y eficiente 🖱️.
3. Los usuarios deben poder guardar los cambios realizados o descartarlos si así lo desean 💾.

## Análisis 🔍

### Pantalla de Edición de Equipos Pokémon

Funcionamiento esperado:

1. El usuario accede a la sección "Editar Equipo" desde la pantalla de gestión de equipos 📝.
2. La pantalla muestra la composición actual del equipo y permite realizar cambios en ella 🔄.
3. El usuario puede añadir o quitar Pokémon y guardar los cambios realizados 📥.

![Modificar equipos Pokémon](../imagenes/modificar_equipo.png)

## Criterios de aceptación ✔️

### Edición de un equipo Pokémon existente

- **Dado**: Que el usuario selecciona un equipo existente para editarlo 📑.
- **Cuando**: Realiza cambios en la composición y elige "Guardar Cambios" ✅.
- **Entonces**: El sistema debe actualizar el equipo con los nuevos cambios y reflejarlos en el perfil del usuario 🔄.

## Diseño 🎨

### Pantalla de Edición de Equipos Pokémon

Para editar un equipo Pokémon existente:

**Request:**
```http
PUT BASE_URL/api/v1/users/{userId}/teams/{teamId}
Content-Type: Application/json
Authorization: Bearer JWT
```

**Body:**
```json
{
  "teamName": "Equipo Legendaria",
  "pokemons": [
    {
      "pokemon_id": 25,
      "pokemon_name": "pikachu",
      "type_element_id": 2
    },
    // ... otros Pokémon
  ]
}
```

**Response: Exitoso statusCode: 200**
```json
{
  "teamId": "team123",
  "message": "Team updated successfully."
}
```

**Response: Error statusCode: 400**
```json
{
  "message": "Error updating team. Please check the provided data."
}
```

## Descripción de las Tablas para la Edición de Equipos Pokémon

### Tabla 'team'
Esta tabla almacena la información de los equipos creados por los usuarios.
- `team_id` INT PK: Un identificador único para cada equipo, que actúa como la clave primaria.
- `team_name` VARCHAR(255): El nombre del equipo, que puede ser editado por el usuario.
- `statistics` INT: Un campo que podría usarse para almacenar estadísticas del equipo, como el número de victorias.
- `user_user_id` INT FK: Una clave foránea que vincula cada equipo a un usuario específico.

### Tabla 'team_pokemon'
Esta tabla relaciona los Pokémon con sus respectivos equipos.
- `team_pokemon_id` INT PK: Un identificador único para cada registro en esta tabla de asociación.
- `pokemon_pokemon_id` INT FK: La clave foránea que referencia a la tabla `pokemon`.
- `team_team_id` INT FK: La clave foránea que referencia a la tabla `team`.

![Estructura de la tabla Team y Team_Pokemon](../imagenes/bd_tus_equipos_pokemon.png)

## Queries SQL para la Edición de un Equipo Pokémon

### Query para Actualizar el Nombre de un Equipo
```sql
UPDATE team
SET team_name = :newTeamName
WHERE team_id = :teamId AND user_user_id = :userId;
```
En esta consulta, `:newTeamName` es el nuevo nombre del equipo, `:teamId` es el ID del equipo que se está editando, y `:userId` es el ID del usuario que posee el equipo.

### Query para Añadir un Pokémon al Equipo
```sql
INSERT INTO team_pokemon (team_team_id, pokemon_pokemon_id)
VALUES (:teamId, :pokemonId)
ON DUPLICATE KEY UPDATE
team_team_id = VALUES(team_team_id), pokemon_pokemon_id = VALUES(pokemon_pokemon_id);
```
Aquí, `:teamId` es el ID del equipo y `:pokemonId` es el ID del Pokémon que se está añadiendo al equipo. La cláusula `ON DUPLICATE KEY UPDATE` es útil si estás reinsertando un Pokémon que ya podría estar en el equipo, aunque podría ajustarse según la lógica específica de tu aplicación.

### Query para Quitar un Pokémon del Equipo
```sql
DELETE FROM team_pokemon
WHERE team_team_id = :teamId AND pokemon_pokemon_id = :pokemonId;
```
Con esta consulta, se eliminaría la asociación de un Pokémon con un equipo. `:teamId` representa el ID del equipo y `:pokemonId` el ID del Pokémon a eliminar.

Estas consultas permitirán realizar cambios en la composición del equipo, incluyendo la actualización del nombre del equipo y la adición o eliminación de Pokémon del mismo. La API procesará los cambios y devolverá una respuesta adecuada al usuario.
