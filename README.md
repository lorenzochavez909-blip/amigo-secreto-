<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amigo Secreto</title>
    <style>
        :root {
            --primary-color: #3b82f6;
            --primary-hover: #2563eb;
            --secondary-color: #10b981;
            --secondary-hover: #059669;
            --background: #f9fafb;
            --card-background: #ffffff;
            --text-color: #1f2937;
            --text-light: #6b7280;
            --border-color: #e5e7eb;
            --error-color: #ef4444;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --radius: 8px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background-color: var(--background);
            color: var(--text-color);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 600px;
            background-color: var(--card-background);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 24px;
            margin-top: 40px;
        }

        h1 {
            text-align: center;
            margin-bottom: 24px;
            color: var(--primary-color);
            font-size: 28px;
        }

        .input-group {
            display: flex;
            gap: 12px;
            margin-bottom: 24px;
        }

        input {
            flex: 1;
            padding: 12px 16px;
            border: 1px solid var(--border-color);
            border-radius: var(--radius);
            font-size: 16px;
            outline: none;
            transition: border-color 0.2s;
        }

        input:focus {
            border-color: var(--primary-color);
        }

        button {
            padding: 12px 20px;
            border: none;
            border-radius: var(--radius);
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--primary-hover);
        }

        .btn-secondary {
            background-color: var(--secondary-color);
            color: white;
            margin-top: 16px;
            width: 100%;
        }

        .btn-secondary:hover {
            background-color: var(--secondary-hover);
        }

        .friends-list {
            margin-bottom: 24px;
        }

        .friends-list h2 {
            font-size: 18px;
            margin-bottom: 12px;
            color: var(--text-light);
        }

        ul {
            list-style-type: none;
            border: 1px solid var(--border-color);
            border-radius: var(--radius);
            overflow: hidden;
        }

        li {
            padding: 12px 16px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        li:last-child {
            border-bottom: none;
        }

        .delete-btn {
            background-color: transparent;
            color: var(--error-color);
            padding: 4px 8px;
            font-size: 14px;
        }

        .delete-btn:hover {
            background-color: rgba(239, 68, 68, 0.1);
        }

        .result {
            text-align: center;
            padding: 20px;
            background-color: rgba(59, 130, 246, 0.05);
            border-radius: var(--radius);
            margin-top: 24px;
            display: none;
        }

        .result h2 {
            color: var(--primary-color);
            margin-bottom: 8px;
        }

        .result p {
            font-size: 24px;
            font-weight: bold;
        }

        .empty-state {
            text-align: center;
            padding: 20px;
            color: var(--text-light);
        }

        @media (max-width: 640px) {
            .input-group {
                flex-direction: column;
            }
            
            .container {
                padding: 16px;
            }
            
            body {
                padding: 16px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎁 Amigo Secreto</h1>
        
        <div class="input-group">
            <input type="text" id="friendName" placeholder="Ingresa el nombre de tu amigo">
            <button class="btn-primary" onclick="agregarAmigo()">Adicionar</button>
        </div>
        
        <div class="friends-list">
            <h2>Lista de amigos</h2>
            <ul id="friendsList">
                <li class="empty-state">Aún no hay amigos agregados</li>
            </ul>
        </div>
        
        <button class="btn-secondary" onclick="sortearAmigo()">Sortear Amigo</button>
        
        <div class="result" id="result">
            <h2>¡El amigo secreto es!</h2>
            <p id="selectedFriend"></p>
        </div>
    </div>

    <script>
        let friends = [];
        
        function agregarAmigo() {
            const input = document.getElementById('friendName');
            const name = input.value.trim();
            
            if (name === '') {
                alert('Por favor, ingresa un nombre válido');
                return;
            }
            
            friends.push(name);
            input.value = '';
            renderFriendsList();
        }
        
        function renderFriendsList() {
            const list = document.getElementById('friendsList');
            
            if (friends.length === 0) {
                list.innerHTML = '<li class="empty-state">Aún no hay amigos agregados</li>';
                return;
            }
            
            list.innerHTML = '';
            friends.forEach((friend, index) => {
                const li = document.createElement('li');
                li.innerHTML = `
                    <span>${friend}</span>
                    <button class="delete-btn" onclick="removeFriend(${index})">Eliminar</button>
                `;
                list.appendChild(li);
            });
        }
        
        function removeFriend(index) {
            friends.splice(index, 1);
            renderFriendsList();
        }
        
        function sortearAmigo() {
            if (friends.length === 0) {
                alert('Debes agregar al menos un amigo primero');
                return;
            }
            
            const randomIndex = Math.floor(Math.random() * friends.length);
            const selected = friends[randomIndex];
            
            document.getElementById('selectedFriend').textContent = selected;
            document.getElementById('result').style.display = 'block';
        }
        
        // Permitir agregar con la tecla Enter
        document.getElementById('friendName').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                agregarAmigo();
            }
        });
    </script>
</body>
</html>
