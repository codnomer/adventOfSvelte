<script>
  import { onMount } from 'svelte';
  import Chart from 'chart.js/auto';

  let heartRates = []; 
  let averageHeartRate = 0;

  let chartCanvas; 
  let chart; 

  onMount(() => {
    async function fetchData() {
      try {
        const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-four.json');
        const data = await response.json();

        
        const currentHeartRate = data.heartRate;
        heartRates = [...heartRates, currentHeartRate];

        averageHeartRate = heartRates.reduce((acc, rate) => acc + rate, 0) / heartRates.length;

        if (chart) {
          chart.data.labels = Array.from({ length: heartRates.length }, (_, i) => i + 1);
          chart.data.datasets[0].data = heartRates;
          chart.update(); 
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    }
    const interval = setInterval(fetchData, 5000);

    fetchData();

    // Clear the interval on component destruction
    return () => clearInterval(interval);
  });

 
  function createChart(node) {
    if (node && !chart) {
      const ctx = node.getContext('2d');
      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: [],
          datasets: [
            {
              label: 'Heart Rate',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 2,
              fill: false,
              data: [],
            },
          ],
        },
        options: {
          scales: {
            x: {
              type: 'linear',
              position: 'bottom',
            },
            y: {
              min: 0,
              max: 200, // Adjust the maximum value based on your data
            },
          },
        },
      });
    }
  }
</script>

<div>
  <h1>Santa's Heart Rate Monitor</h1>

  <div>
    <p>Live Heart Rate: {heartRates.length > 0 ? heartRates[heartRates.length - 1] : 'Loading...'}</p>
    <p>Average Heart Rate: {averageHeartRate.toFixed(2)}</p>
  </div>

  <div>
    <h2>Historical View</h2>
    <canvas bind:this={chartCanvas} use:createChart />
  </div>
</div>
