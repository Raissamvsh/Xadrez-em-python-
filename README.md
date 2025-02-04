<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xadrez Online</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #1e1e1e;
            margin: 0;
            font-family: Arial, sans-serif;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(8, 80px);
            grid-template-rows: repeat(8, 80px);
            border: 8px solid #333;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
        }

        .square {
            width: 80px;
            height: 80px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 40px;
            font-weight: bold;
            cursor: grab;
            user-select: none;
        }

        .dark {
            background-color: #769656;
        }

        .light {
            background-color: #eeeed2;
        }

        .square.dragging {
            opacity: 0.5;
        }
    </style>
</head>
<body>
    <div class="board" id="chessboard"></div>

    <script>
        function createBoard() {
            const board = document.getElementById("chessboard");
            const pieces = {
                0: "♜♞♝♛♚♝♞♜",
                1: "♟♟♟♟♟♟♟♟",
                6: "♙♙♙♙♙♙♙♙",
                7: "♖♘♗♕♔♗♘♖"
            };

            for (let row = 0; row < 8; row++) {
                for (let col = 0; col < 8; col++) {
                    let square = document.createElement("div");
                    square.className = "square " + ((row + col) % 2 === 0 ? "light" : "dark");
                    square.dataset.row = row;
                    square.dataset.col = col;

                    if (pieces[row]) {
                        square.innerText = pieces[row][col];
                        square.dataset.piece = pieces[row][col];
                        square.draggable = true;
                        square.addEventListener("dragstart", dragStart);
                        square.addEventListener("dragover", dragOver);
                        square.addEventListener("drop", drop);
                    }

                    board.appendChild(square);
                }
            }
        }

        let draggedPiece = null;

        function dragStart(event) {
            draggedPiece = event.target;
            setTimeout(() => event.target.classList.add("dragging"), 0);
        }

        function dragOver(event) {
            event.preventDefault();
        }

        function drop(event) {
            event.preventDefault();
            if (event.target.classList.contains("square")) {
                event.target.innerText = draggedPiece.innerText;
                event.target.dataset.piece = draggedPiece.dataset.piece;
                draggedPiece.innerText = "";
                draggedPiece.dataset.piece = "";
            }
            draggedPiece.classList.remove("dragging");
        }

        createBoard();
    </script>
</body>
</html>
