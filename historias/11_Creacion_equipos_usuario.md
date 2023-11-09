# Historia: Creación de Equipos Pokémon 🛠️

- **Yo como**: Usuario jugador 🧢
- **Quiero**: Poder crear y personalizar equipos Pokémon 📝
- **Para**: Prepararme para batallas y torneos competitivos 🥇.

## Pendientes de definición 📌

1. ¿Cuántos equipos puede tener un usuario? ¿Habrá un límite?
   R. Se definirá el límite de equipos por usuario tras la primera fase de pruebas.

## Especificación de requerimientos 📐

1. Los usuarios deben poder crear un nuevo equipo.
2. Cada equipo puede estar compuesto por un máximo de 6 Pokémon, siguiendo las reglas estándar de la franquicia 🎮.
3. Los usuarios deben poder seleccionar Pokémon de una base de datos integrada, como PokeAPI, para añadirlos a sus equipos 🌐.
4. La interfaz para la creación de equipos debe ser intuitiva y guiar al usuario a través del proceso paso a paso 🖌️.

## Análisis 🕵️‍♂️

### Pantalla de Creación de Nuevo Equipo Pokémon

Funcionamiento esperado:

1. El usuario selecciona la opción "Crear Nuevo Equipo" en la interfaz principal 🆕.
2. El usuario puede buscar y seleccionar Pokémon para añadir a su equipo 📝.
3. Una vez completado, el usuario puede guardar el equipo en su perfil 📥.

![Alt text](../imagenes/crear.png)

## Criterios de aceptación ✅

### Creación de un nuevo equipo Pokémon

- **Dado**: Que el usuario ha iniciado sesión en la aplicación 🌐.
- **Cuando**: Elige la opción de "Crear Nuevo Equipo" y completa el proceso de selección y personalización de Pokémon 🖱️.
- **Entonces**: El sistema debe guardar el nuevo equipo en el perfil del usuario 📝.

## Diseño 🎨

### Pantalla de Creación de Nuevo Equipo Pokémon

Para crear un nuevo equipo Pokémon:

**Request:**
```http
POST BASE_URL/api/v1/users/{userId}/teams
Content-Type: Application/json
Authorization: Bearer JWT
```

**Body:**
```json
{
  "teamName": "Equipo 1",
  "pokemons": [
    {
      "pokemonId": "150",
      "nickname": "Mewtwo",
      "Type1": "Psychic",  // Tipo primario
      }
    },
    // ... otros Pokémon
  ]
}
```

**Response: Exitoso statusCode: 201**
```json
{
  "teamId": "newTeam1",
  "message": "Team created successfully."
}
```

**Response: Error statusCode: 400**
```json
{
  "message": "Error creating team. Please check the provided data."
}
```

Claro, aquí está la información en formato Markdown (`.md`) para la documentación de la creación de un nuevo equipo Pokémon y los queries SQL correspondientes:

```markdown
## Descripciones de las Tablas para la Creación de Equipos

### Tabla 'team'
Esta tabla almacena los equipos de los usuarios. Al crear un nuevo equipo, se insertará una nueva fila con los siguientes campos:
- `team_name`: Nombre del nuevo equipo Pokémon.
- `user_user_id`: ID del usuario que está creando el equipo, actuando como una llave foránea.

### Tabla 'team_pokemon'
Después de crear un nuevo equipo, esta tabla se utilizará para asociar Pokémon individuales con el equipo correspondiente. Los campos implicados en la inserción son:
- `team_team_id`: ID del equipo recién creado.
- `pokemon_pokemon_id`: ID de cada Pokémon que será agregado al equipo.

## Queries SQL para la Operación de Creación de Equipo

### Paso 1: Insertar un Nuevo Equipo
```sql
INSERT INTO team (team_name, user_user_id) VALUES (:teamName, :userId);
```
Aquí, `:teamName` representa el nombre del nuevo equipo y `:userId` el ID del usuario.

### Paso 2: Insertar Pokémon en el Equipo
Para cada Pokémon que se quiera añadir al equipo, se ejecuta:
```sql
INSERT INTO team_pokemon (team_team_id, pokemon_pokemon_id) VALUES (:teamId, :pokemonId);
```
Donde `:teamId` es el ID del equipo y `:pokemonId` el ID del Pokémon a insertar.
