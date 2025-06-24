# Tic-Tac-Toe
Tic Tac Toe using Mern Stack Development
#CODE FOR TIC TAC TOE
import React, { useState } from 'react';

const initialBoard = Array(9).fill('');

const TicTacToe = () => {
  const [board, setBoard] = useState(initialBoard);
  const [turn, setTurn] = useState('X');
  const [winner, setWinner] = useState(null);
  const [isDraw, setIsDraw] = useState(false);

  const handleClick = (index) => {
    if (board[index] || winner) return; // Ignore if cell filled or game over

    const newBoard = [...board];
    newBoard[index] = turn;
    setBoard(newBoard);

    if (checkWinner(newBoard)) {
      setWinner(turn);
      setIsDraw(false);
    } else if (newBoard.every(cell => cell !== '')) {
      setIsDraw(true);
    } else {
      setTurn(turn === 'X' ? 'O' : 'X');
      setIsDraw(false);
    }
  };

  const checkWinner = (b) => {
    const winPatterns = [
      [0,1,2],[3,4,5],[6,7,8], // rows
      [0,3,6],[1,4,7],[2,5,8], // cols
      [0,4,8],[2,4,6]          // diagonals
    ];

    for (let pattern of winPatterns) {
      const [a, b1, c] = pattern;
      if (b[a] && b[a] === b[b1] && b[a] === b[c]) {
        return true;
      }
    }
    return false;
  };

  const resetGame = () => {
    setBoard(initialBoard);
    setTurn('X');
    setWinner(null);
    setIsDraw(false);
  };

  return (
    <div style={{ textAlign: 'center', marginTop: 40 }}>
      <h2>Tic Tac Toe</h2>
      <div
        style={{
          display: 'grid',
          gridTemplateColumns: 'repeat(3, 100px)',
          gridGap: 10,
          justifyContent: 'center',
          marginBottom: 20
        }}
      >
        {board.map((cell, i) => (
          <div
            key={i}
            onClick={() => handleClick(i)}
            style={{
              width: 100,
              height: 100,
              border: '2px solid #333',
              borderRadius: 8,
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'center',
              fontSize: 48,
              cursor: winner || isDraw ? 'default' : 'pointer',
              backgroundColor: cell === 'X' ? '#d1e7dd' : cell === 'O' ? '#f8d7da' : '#fff',
              userSelect: 'none'
            }}
          >
            {cell}
          </div>
        ))}
      </div>

      {winner && <h3 style={{ color: 'green' }}>Winner: {winner}</h3>}
      {isDraw && <h3 style={{ color: 'orange' }}>It's a Draw!</h3>}

      {(winner || isDraw) && (
        <button
          onClick={resetGame}
          style={{
            marginTop: 20,
            padding: '10px 20px',
            fontSize: 16,
            cursor: 'pointer',
            borderRadius: 5,
            border: 'none',
            backgroundColor: '#0d6efd',
            color: '#fff'
          }}
        >
          Restart Game
        </button>
      )}

      {!winner && !isDraw && (
        <h4>Turn: {turn}</h4>
      )}
    </div>
  );
};

export default TicTacToe;
