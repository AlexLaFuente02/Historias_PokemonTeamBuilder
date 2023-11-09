# Historia: Visualización de Equipos Pokémon 📜

- **Yo como**: Usuario jugador 🎮
- **Quiero**: Poder visualizar todos mis equipos Pokémon 🧐
- **Para**: Gestionar y organizar mis equipos de batalla 🏆.

## Pendientes de definición 🤔

1. ¿Se debería mostrar un ejemplo de equipo Pokemón si el usuario no tiene equipos?
   R. Alex dice que se verá la integración de esta opción de acuerdo a la opinión de los primeros usuarios después de desplegar el proyecto.

## Especificación de requerimientos ✅

1. La pantalla debe mostrar todos los equipos Pokémon del usuario 📲.
2. Cada equipo debe presentar un resumen con los nombres y las imágenes de los Pokémon que lo componen 🖼️.
3. Debe haber una opción para seleccionar un equipo y verlo en detalle 🔍.
4. La interfaz debe ser clara y fácil de usar en dispositivos móviles y de escritorio 💻📱.

## Análisis 🕵️

### Pantalla de Visualización de Equipos Pokémon

Funcionamiento esperado:

1. El usuario accede a la sección "Mis Equipos" desde el menú principal 📋.
2. La pantalla muestra todos los equipos Pokémon que el usuario ha creado 🌟 o le permite crear uno si es su primera vez.
3. El usuario puede seleccionar cualquier equipo para verlo en detalle o editar su composición ✏️.

![Pantalla tus equipos pokemón](../imagenes/tus_equipos_pokemon.png)

## Criterios de aceptación 🎯

### Visualización de equipos existentes 📊

- **Dado**: Que el usuario ha iniciado sesión y tiene al menos un equipo creado.
- **Cuando**: Accede a la sección "Mis Equipos".
- **Entonces**: El sistema debe mostrar todos los equipos Pokémon del usuario.

### Selección de un equipo para ver detalles 🔎

- **Dado**: Que el usuario está en la pantalla "Mis Equipos".
- **Cuando**: Selecciona un equipo.
- **Entonces**: El sistema debe llevar al usuario a la pantalla de detalle de ese equipo.

## Diseño 🎨

### Pantalla de Visualización de Equipos Pokémon

Para obtener la lista de equipos del usuario:

**Request:**
```http
GET BASE_URL/api/v1/users/{userId}/teams
Accept: Application/json
Authorization: Bearer JWT
```

**Response: Exitoso statusCode: 200**
```json
[
  {
    "teamId": "team123",
    "teamName": "Equipo Alfa",
    "pokemons": [
      {
        "pokemonId": "001",
        "name": "Bulbasaur",
        "sprite": "url-de-la-imagen-bulbasaur.png"
      },
      // ... otros Pokémon
    ]
  },
  // ... otros equipos
]
```

**Response: No hay equipos statusCode: 404**
```json
{
  "message": "No teams found for this user."
}
```

## Descripción de las Tablas:

![Captura tablas](../imagenes/bd_tus_equipos_pokemon.png)


**Tabla 'team'**
- Esta tabla almacena los equipos de Pokémon. Cada equipo tiene un `id` único, un `team_name` que identifica el nombre del equipo, y una columna `statistics` para almacenar posibles estadísticas del equipo. La columna `user_user_id` es una llave foránea que vincula cada equipo a un usuario específico.

**Tabla 'team_pokemon'**
- Funciona como una tabla de asociación entre 'team' y 'pokemon', permitiendo que un equipo tenga múltiples Pokémon. La `team_pokemon_id` es una llave primaria única para cada entrada. `team_team_id` vincula cada Pokémon al equipo correspondiente y `pokemon_pokemon_id` vincula al Pokémon específico de la tabla 'pokemon'.

**Tabla 'pokemon'**
- Contiene los detalles de cada Pokémon. `pokemon_id` es la llave primaria única para cada Pokémon, `pokemon_name` almacena el nombre, y `type_element_type_element_id` puede usarse para asociar cada Pokémon con su tipo elemental.

## Consulta SQL para Obtener Todos los Equipos de un Usuario:

Para obtener todos los equipos de un usuario, realizaríamos una consulta SQL que se uniría a las tres tablas. Aquí está la consulta:

```sql
SELECT t.team_id, t.team_name, p.pokemon_id, p.pokemon_name
FROM team t
JOIN team_pokemon tp ON t.id = tp.team_team_id
JOIN pokemon p ON tp.pokemon_pokemon_id = p.pokemon_id
WHERE t.user_user_id = :userId;
```

En esta consulta, `:userId` sería el parámetro que representa el ID del usuario cuyos equipos queremos recuperar. Esta consulta selecciona el ID y el nombre del equipo de la tabla 'team', y para cada equipo, selecciona los Pokémon que contiene, incluyendo sus ID y nombres de la tabla 'pokemon'. La unión se realiza mediante los ID correspondientes en la tabla 'team_pokemon'.
