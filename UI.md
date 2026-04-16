Quantum Game API Spec
Overview
The frontend acts as a "dumb terminal" that visualizes the game state and collects user intent. All quantum logic, circuit generation, and state collapse calculations are handled by the backend.

Base URL: /api/v1/game (or your configured backend route)

1. Initialize Game Session

Endpoint: POST /init

Description: Starts a new game session and resets the tree position.

Request Body(JSON): (Empty or optional config)
{
  "bounds": [-3, 3] 
}

Response(JSON):
{
  "session_id": "uuid-string",
  "position": 0,
  "step": 0,
  "message": "Game initialized."
}

2. Place Token & Execute Move

Endpoint: POST /move

Description: Sends the player's chosen token to the backend, applying the corresponding quantum gate and measuring the state to determine the next position.

Request Body(JSON):
{
  "session_id": "uuid-string",
  "token_type": "Superposition", // Options: "Superposition", "Entanglement", "Right", "Left"
  "control_qubit": null // (Optional) Int. If using 'Entanglement', specify the control qubit index. Defaults to (current_step - 1) if null.
}

Response(JSON):
{
  "step": 1,
  "measured_state": "|0⟩", // "|0⟩" (Up-Left) or "|1⟩" (Up-Right)
  "direction": "Up-Left",
  "new_position": -1,
  "is_game_over": false,
  "circuit_diagram_url": "/static/circuits/uuid_step1.png", // URL to the generated matplotlib circuit
  "message": "State collapsed to |0⟩."
}