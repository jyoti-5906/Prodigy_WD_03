(function() {
  const board = document.querySelector('.board');
  const cells = document.querySelectorAll('.cell');
  const status = document.querySelector('.status');
  const resetBtn = document.getElementById('resetBtn');

  let currentPlayer = 'X';
  let gameActive = true;
  let boardState = ['', '', '', '', '', '', '', '', ''];

  const winningConditions = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
  ];

  function handleCellClick(e) {
    const clickedCell = e.target;
    const clickedIndex = parseInt(clickedCell.getAttribute('data-index'));
    if (boardState[clickedIndex] !== '' || !gameActive) return;
    updateCell(clickedCell, clickedIndex);
    checkResult();
  }

  function updateCell(cell, index) {
    boardState[index] = currentPlayer;
    cell.textContent = currentPlayer;
    cell.classList.add('taken');
  }

  function changePlayer() {
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    status.textContent = `Player ${currentPlayer}'s turn`;
  }

  function checkResult() {
    let roundWon = false;
    for (let i = 0; i < winningConditions.length; i++) {
      const [a, b, c] = winningConditions[i];
      if (boardState[a] === '' || boardState[b] === '' || boardState[c] === '') continue;
      if (boardState[a] === boardState[b] && boardState[b] === boardState[c]) {
        roundWon = true;
        break;
      }
    }
    if (roundWon) {
      status.textContent = `Player ${currentPlayer} has won! 🎉`;
      gameActive = false;
      return;
    }
    if (!boardState.includes('')) {
      status.textContent = 'Game ended in a draw.';
      gameActive = false;
      return;
    }
    changePlayer();
  }

  function resetGame() {
    currentPlayer = 'X';
    gameActive = true;
    boardState = ['', '', '', '', '', '', '', '', ''];
    status.textContent = `Player ${currentPlayer}'s turn`;
    cells.forEach(cell => {
      cell.textContent = '';
      cell.classList.remove('taken');
    });
  }

  cells.forEach(cell => cell.addEventListener('click', handleCellClick));
  resetBtn.addEventListener('click', resetGame);
})();
