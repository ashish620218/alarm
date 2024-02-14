<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
  
    let currentTime = new Date();
    let alarmSound :HTMLAudioElement
  
    const updateTime = () => {
      currentTime = new Date();
    }; 
  
    const interval = setInterval(updateTime, 1000);
  
    onMount(() => {
        alarmSound = new Audio('/music/alerm.wav');
      updateTime();
    });
  
    onDestroy(() => { 
      clearInterval(interval);
    });
  
    function setAlarm() {
      const alarmTimeInput = document.getElementById('alarmTime');
      //@ts-ignore
      const selectedTime = alarmTimeInput.value;
      const currentTime = new Date();
      const alarmDateTime = new Date(currentTime.toDateString() + ' ' + selectedTime);
      //@ts-ignore
      const timeUntilAlarm = alarmDateTime - currentTime;

      if (timeUntilAlarm <= 0) {
        alert('Please select a future time for the alarm.');
        return;
      }
  
      setTimeout(() => {
        alarmSound.play(); 
       // alert('Alarm!');
      }, timeUntilAlarm);
    }
  
    function clearAlarm() {
      // Implement logic to clear alarm if needed
      alert('Alarm cleared!');
    }
  </script>
  
  <div class="head">
    <div class="container">
      <h1>Alarm Clock</h1>
      <div>
        <h2 id="currentTime">Current Time: {currentTime.toLocaleTimeString()}</h2>
      </div>
      <div>
        <label for="alarmTime">Set Alarm:</label>
        <input type="time" id="alarmTime">
        <button on:click={setAlarm}>Set</button>
        <button on:click={clearAlarm}>Clear Alarm</button>
      </div>
    </div>
  </div>
  
  <style>
    .head {
      font-family: Arial, sans-serif;
      text-align: center;
    }
  
    .container {
      margin-top: 50px;
    }
  
    #currentTime {
      font-size: 24px;
    }
  
    input[type="time"] {
      margin: 20px;
      padding: 10px;
    }
  
    button {
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
      margin: 10px;
    }
  </style>
  