# core-nexus
import React, { useState, useEffect, useRef } from 'react';
import { FaMoon, FaSun, FaSmile } from 'react-icons/fa';
import { motion } from 'framer-motion';

export default function ArbiChat() {
  const [darkMode, setDarkMode] = useState(true);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const messagesEndRef = useRef(null);

  const toggleDarkMode = () => setDarkMode(!darkMode);

  const sendMessage = () => {
    if (input.trim()) {
      setMessages([...messages, { user: 'You', text: input, id: Date.now() }]);
      setInput('');
    }
  };

  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  const containerClasses = darkMode ? 'bg-gradient-to-br from-blue-900 to-black text-white' : 'bg-white text-black';

  return (
    <div className={`min-h-screen flex flex-col ${containerClasses} transition-all duration-500`}>
      {/* Navbar */}
      <nav className="flex justify-between items-center p-4 backdrop-blur-md bg-white/10">
        <h1 className="text-2xl font-bold">ArbiChat</h1>
        <button onClick={toggleDarkMode} className="p-2 rounded-full hover:bg-white/20">
          {darkMode ? <FaSun size={20} /> : <FaMoon size={20} />}
        </button>
      </nav>

      {/* Chat Area */}
      <div className="flex flex-1 overflow-hidden">
        {/* User Status Panel */}
        <div className="hidden md:flex flex-col w-1/4 p-4 border-r border-white/20">
          <div className="glass p-4 rounded-2xl mb-4">
            <h2 className="font-semibold mb-2">Status</h2>
            <p className="text-sm">Connected to random stranger...</p>
          </div>
        </div>

        {/* Messages Area */}
        <div className="flex-1 p-4 flex flex-col">
          <div className="flex-1 overflow-y-auto space-y-4">
            {messages.map(msg => (
              <motion.div
                key={msg.id}
                initial={{ opacity: 0, y: 10 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.3 }}
                className="glass p-3 rounded-xl max-w-sm"
              >
                <div className="flex items-center space-x-2">
                  <div className="w-8 h-8 rounded-full bg-gradient-to-tr from-cyan-400 to-blue-600" />
                  <span className="font-semibold">{msg.user}</span>
                </div>
                <p className="mt-2">{msg.text}</p>
              </motion.div>
            ))}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Box */}
          <div className="mt-4 flex items-center bg-white/10 backdrop-blur-md rounded-2xl p-2">
            <button className="p-2"><FaSmile size={20} /></button>
            <input
              type="text"
              value={input}
              onChange={e => setInput(e.target.value)}
              onKeyDown={e => e.key === 'Enter' && sendMessage()}
              placeholder="Type a message..."
              className="flex-1 bg-transparent outline-none p-2 text-base"
            />
            <button
              onClick={sendMessage}
              className="ml-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-2xl"
            >Send</button>
          </div>
        </div>
      </div>

      {/* Start Chatting Button on Homepage */}
      {messages.length === 0 && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          className="absolute inset-0 flex items-center justify-center"
        >
          <button
            onClick={() => setMessages([{ user: 'Stranger', text: 'Hi there! 👋', id: Date.now() }])}
            className="bg-gradient-to-r from-blue-500 to-cyan-400 hover:from-cyan-400 hover:to-blue-500 text-white px-8 py-4 rounded-full text-xl shadow-lg backdrop-blur-md"
          >
            Start Chatting
          </button>
        </motion.div>
      )}
    </div>
  );
}
