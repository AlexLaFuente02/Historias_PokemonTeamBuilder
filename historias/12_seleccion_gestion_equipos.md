# Historia: Selección y Gestión de Pokémon en Equipos 🔄

- **Yo como**: Usuario Jugador 🎽
- **Quiero**: Poder seleccionar y gestionar los Pokémon dentro de mis equipos 📊
- **Para**: Optimizar mis estrategias y tener una alineación balanceada para cada batalla 🏆.

## Pendientes de definición 🗒️

1. ¿Cómo manejaremos los Pokémon duplicados en un equipo?
   R. Se permitirán duplicados siempre y cuando cumplan con las reglas de torneos específicos.

## Especificación de requerimientos 📋

1. Los usuarios deben poder añadir o quitar Pokémon de sus equipos existentes 🔄.
2. Debe ser posible reordenar los Pokémon dentro de un equipo para planificar la estrategia de batalla 📈.
3. Los usuarios deben poder ver un resumen de las estadísticas de combate y habilidades de cada Pokémon en el equipo 📊.
4. La interfaz debe permitir la fácil selección de Pokémon de la integracion con pokeapi y su colocación en el equipo deseado 🗃️.
5. Se debe proporcionar una opción para guardar cualquier cambio realizado en la configuración del equipo 💾.

## Análisis 🕵️‍♀️

### Pantalla de Gestión de Equipos Pokémon

Funcionamiento esperado:

1. El usuario accede a la sección "Gestionar Equipos" desde su perfil 🛠️.
2. Se muestra una lista de equipos existentes con la opción de editarlos ✏️.
3. Al seleccionar un equipo, el usuario puede añadir o quitar Pokémon, así como ajustar su orden y configuración 🎚️.
4. Los cambios se pueden guardar o descartar antes de salir de la pantalla de gestión 🚪.

![Alt text](../imagenes/seleccion.png)

## Criterios de aceptación ✔️

Gherkin

### Gestión de Pokémon en un equipo

- **Dado**: Que el usuario ha seleccionado un equipo para gestionar 📝.
- **Cuando**: Añade, quita o reordena los Pokémon y selecciona "Guardar Cambios" 💾.
- **Entonces**: El sistema debe actualizar el equipo con los cambios realizados ✅.

## Diseño 🖌️

### Pantalla de Gestión de Equipos Pokémon

Para actualizar la composición de un equipo Pokémon:

**Request:**
```http
PUT BASE_URL/api/v1/users/{userId}/teams/{teamId}
Content-Type: Application/json
Authorization: Bearer JWT
```

**Body:**
```json
{
  "pokemons": [
    {
      "pokemonId": "025",
      "nickname": "Pikachu",
      "moves": ["Thunderbolt", "Quick Attack", "Iron Tail", "Electro Ball"],
      "heldItem": "Light Ball",
      "stats": {
        "hp": 35,
        "attack": 55,
        "defense": 40,
        // ... otros stats
      }
    },
    // ... ajustes para otros Pokémon
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
