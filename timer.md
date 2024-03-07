```jsx
import React, { useState, useRef, useEffect } from 'react';

function Timer() {
  const [time, setTime] = useState(0);
  const [timerOn, setTimerOn] = useState(false);
  const intervalRef = useRef(null);

  const startTimer = () => {
    if (!timerOn && time > 0) {
      setTimerOn(true);
      intervalRef.current = setInterval(() => {
        setTime((prevTime) => prevTime - 1);
      }, 1000);
    }
  };

  const pauseTimer = () => {
    if (timerOn) {
      clearInterval(intervalRef.current);
      setTimerOn(false);
    }
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
    setTimerOn(false);
    setTime(0);
  };

  useEffect(() => {
    return () => clearInterval(intervalRef.current);
  }, []);

  return (
    <div>
      <h2>{time} Seconds</h2>
      <input
        type="number"
        value={time}
        onChange={(e) => setTime(parseInt(e.target.value))}
        disabled={timerOn}
      />
      <button onClick={startTimer} disabled={timerOn}>Start</button>
      <button onClick={pauseTimer} disabled={!timerOn}>Pause</button>
      <button onClick={stopTimer}>Stop</button>
    </div>
  );
}

export default Timer;
```