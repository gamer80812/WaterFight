# WaterFight
import React, { useState, useEffect } from 'react';
import { createRoot } from 'react-dom/client';
import './index.css';

function SplashinApp() {
  const [players, setPlayers] = useState([]);
  const [name, setName] = useState('');
  const [target, setTarget] = useState('');
  const [chat, setChat] = useState([]);
  const [message, setMessage] = useState('');
  const [startTime, setStartTime] = useState(null);

  const addPlayer = () => {
    if (!name) return alert("Please enter player name");

    if (target && !players.some(p => p.name === target)) {
      return alert("Target must be an existing player");
    }

    setPlayers([...players, { name, target: target || '—', status: 'alive' }]);
    setName('');
    setTarget('');
  };

  const toggleStatus = (index) => {
    const updated = [...players];
    updated[index].status = updated[index].status === 'alive' ? 'out' : 'alive';
    setPlayers(updated);
  };

  const deletePlayer = (index) => {
    const updated = [...players];
    updated.splice(index, 1);
    setPlayers(updated);
  };

  const sendMessage = () => {
    if (!message.trim()) return;
    setChat([...chat, message]);
    setMessage('');
  };

  const startGame = () => {
    setStartTime(Date.now());
  };

  const getWinner = () => {
    const alive = players.filter(p => p.status === 'alive');
    return alive.length === 1 ? alive[0].name : null;
  };

  const elapsedTime = startTime ? Math.floor((Date.now() - startTime) / 1000) : 0;

  return (
    <div className="p-4 max-w-2xl mx-auto text-white bg-gradient-to-b from-sky-900 to-slate-900 min-h-screen">
      <h1 className="text-4xl font-bold mb-6 text-center">Splashin Web</h1>

      <div className="grid grid-cols-1 gap-2 mb-4">
        <input
          className="border p-2 rounded text-black"
          placeholder="Player Name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <input
          className="border p-2 rounded text-black"
          placeholder="Target Name (optional)"
          value={target}
          onChange={(e) => setTarget(e.target.value)}
        />
        <div className="flex gap-2">
          <button className="bg-blue-500 text-white py-2 px-4 rounded" onClick={addPlayer}>Add Player</button>
          <button className="bg-green-500 text-white py-2 px-4 rounded" onClick={startGame}>Start Game</button>
        </div>
      </div>

      {startTime && (
        <p className="mb-4 text-center">⏱ Game time: {elapsedTime}s</p>
      )}

      {getWinner() && (
        <div className="mb-4 text-center bg-yellow-300 text-black p-3 rounded font-bold">
          🏆 Winner: {getWinner()} 🎉
        </div>
      )}

      <div className="space-y-2 mb-6">
        {players.map((player, index) => (
          <div
            key={index}
            className={`flex justify-between items-center p-3 rounded border ${player.status === 'out' ? 'bg-red-100 text-black' : 'bg-green-100 text-black'}`}
          >
            <div onClick={() => toggleStatus(index)} className="cursor-pointer flex-1">
              <strong>{player.name}</strong> → {player.target} ({player.status})
            </div>
            <button onClick={() => deletePlayer(index)} className="ml-4 text-red-600 font-bold">❌</button>
          </div>
        ))}
      </div>

      <div className="bg-white text-black rounded p-4">
        <h2 className="text-xl font-bold mb-2">💬 Chat</h2>
        <div className="max-h-40 overflow-y-auto border p-2 mb-2 bg-gray-100 rounded">
          {chat.map((msg, i) => (
            <div key={i} className="mb-1">{msg}</div>
          ))}
        </div>
        <div className="flex gap-2">
          <input
            className="border p-2 rounded flex-1"
            placeholder="Type a message"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
          />
          <button className="bg-blue-600 text-white px-4 rounded" onClick={sendMessage}>Send</button>
        </div>
      </div>
    </div>
  );
}

const container = document.getElementById('root');
const root = createRoot(container);
root.render(<SplashinApp />);
