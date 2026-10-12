# jogo-da-memoria```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Memória das Cores</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #f0f4f8;
            text-align: center;
            margin: 0;
            padding: 20px;
        }

        h1 {
            color: #333;
        }

        p {
            color: #555;
        }

        .jogo {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            gap: 12px;
            justify-content: center;
            margin: 25px auto;
        }

        .carta {
            width: 100px;
            height: 100px;
            background-color: #444;
            border: 3px solid white;
            border-radius: 12px;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .carta:hover {
            transform: scale(1.05);
        }

        .carta.acertada {
            border: 3px solid #28a745;
            cursor: default;
        }

        button {
            padding: 12px 24px;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            background-color: #007bff;
            color: white;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        #mensagem {
            font-size: 20px;
            font-weight: bold;
            color: #228b22;
        }

        @media (max-width: 450px) {
            .jogo {
                grid-template-columns: repeat(4, 70px);
                gap: 8px;
            }

            .carta {
                width: 70px;
                height: 70px;
            }
        }
    </style>
</head>

<body>

    <h1>🎨 Jogo da Memória das Cores</h1>

    <p>Encontre os 8 pares de cores iguais!</p>

    <p>Jogadas: <span id="jogadas">0</span></p>

    <div class="jogo" id="jogo"></div>

    <p id="mensagem"></p>

    <button onclick="reiniciarJogo()">Reiniciar jogo</button>

    <script>
        const cores = [
            "yellow",
            "red",
            "blue",
            "green",
            "orange",
            "pink",
            "white",
            "black"
        ];

        let primeiraCarta = null;
        let segundaCarta = null;
        let bloqueado = false;
        let jogadas = 0;
        let paresEncontrados = 0;

        const tabuleiro = document.getElementById("jogo");
        const contador = document.getElementById("jogadas");
        const mensagem = document.getElementById("mensagem");

        function iniciarJogo() {
            tabuleiro.innerHTML = "";

            primeiraCarta = null;
            segundaCarta = null;
            bloqueado = false;
            jogadas = 0;
            paresEncontrados = 0;

            contador.textContent = jogadas;
            mensagem.textContent = "";

            // Cria dois cartões de cada cor
            const cartas = [...cores, ...cores];

            // Embaralha as cartas
            cartas.sort(() => Math.random() - 0.5);

            cartas.forEach((cor) => {
                const carta = document.createElement("button");

                carta.classList.add("carta");
                carta.dataset.cor = cor;
                carta.setAttribute("aria-label", "Carta fechada");
                carta.style.backgroundColor = "#444";

                carta.addEventListener("click", () => virarCarta(carta));

                tabuleiro.appendChild(carta);
            });
        }

        function virarCarta(carta) {
            if (
                bloqueado ||
                carta === primeiraCarta ||
                carta.classList.contains("acertada") ||
                carta.dataset.aberta === "sim"
            ) {
                return;
            }

            carta.style.backgroundColor = carta.dataset.cor;
            carta.dataset.aberta = "sim";
            carta.setAttribute("aria-label", carta.dataset.cor);

            if (primeiraCarta === null) {
                primeiraCarta = carta;
                return;
            }

            segundaCarta = carta;
            jogadas++;

            contador.textContent = jogadas;

            if (
                primeiraCarta.dataset.cor ===
                segundaCarta.dataset.cor
            ) {
                primeiraCarta.classList.add("acertada");
                segundaCarta.classList.add("acertada");

                paresEncontrados++;

                primeiraCarta = null;
                segundaCarta = null;

                if (paresEncontrados === 8) {
                    mensagem.textContent =
                        "🎉 Parabéns! Você encontrou todos os pares!";
                }
            } else {
                bloqueado = true;

                setTimeout(() => {
                    primeiraCarta.style.backgroundColor = "#444";
                    segundaCarta.style.backgroundColor = "#444";

                    primeiraCarta.dataset.aberta = "nao";
                    segundaCarta.dataset.aberta = "nao";

                    primeiraCarta.setAttribute("aria-label", "Carta fechada");
                    segundaCarta.setAttribute("aria-label", "Carta fechada");

                    primeiraCarta = null;
                    segundaCarta = null;
                    bloqueado = false;
                }, 800);
            }
        }

        function reiniciarJogo() {
            iniciarJogo();
        }

        iniciarJogo();
    </script>

</body>
</html>
```
