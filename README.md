<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daily Budget Rules Calculator</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Inter', sans-serif;
      background-color: #f8fafc;
    }
    .gradient-card {
      background: linear-gradient(135deg, #ffffff 0%, #f1f5f9 100%);
    }
    .calculator-header {
      background: linear-gradient(135deg, #0ea5e9 0%, #3b82f6 100%);
    }
    .input-style {
      transition: all 0.2s;
    }
    .input-style:focus {
      ring: 2px;
      ring-color: #3b82f6;
      border-color: #3b82f6;
    }
  </style>
</head>
<body class="min-h-screen">
  <!-- Header -->
  <div class="calculator-header text-white py-6 px-4 mb-8">
    <div class="max-w-7xl mx-auto">
      <h1 class="text-3xl font-bold">Daily Budget Rules Calculator</h1>
      <p class="text-blue-100 mt-2">Amazon PPC Budget Adjustment Tool</p>
    </div>
  </div>

  <div class="max-w-7xl mx-auto px-4">
    <div class="grid md:grid-cols-2 gap-8">
      <!-- Decrease Budget Section -->
      <div class="gradient-card rounded-2xl shadow-lg p-6">
        <div class="flex items-center mb-6">
          <div class="w-10 h-10 rounded-full bg-red-100 flex items-center justify-center mr-3">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
            </svg>
          </div>
          <h2 class="text-xl font-semibold text-gray-800">Decrease Budget</h2>
        </div>

        <div class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Decrease Percentage (%)</label>
            <input type="number" id="decreasePercentage" value="40" min="0" max="100"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg input-style"
              onchange="calculateDecrease()"
            >
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Input Budget ($)</label>
            <input type="number" id="decreaseInputBudget" value="100"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg input-style"
              onchange="calculateDecrease()"
            >
          </div>

          <div class="bg-gray-50 rounded-lg p-4 space-y-3">
            <div>
              <label class="text-sm text-gray-600">Budget After Decrease</label>
              <div id="decreasedBudget" class="text-xl font-semibold text-gray-900">$60.00</div>
            </div>
            <div>
              <label class="text-sm text-gray-600">Required Increase to Return (%)</label>
              <div id="requiredIncrease" class="text-xl font-semibold text-blue-600">66.67%</div>
            </div>
            <div>
              <label class="text-sm text-gray-600">Returning Budget</label>
              <div id="returnedDecreaseBudget" class="text-xl font-semibold text-gray-900">$100.00</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Increase Budget Section -->
      <div class="gradient-card rounded-2xl shadow-lg p-6">
        <div class="flex items-center mb-6">
          <div class="w-10 h-10 rounded-full bg-green-100 flex items-center justify-center mr-3">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7" />
            </svg>
          </div>
          <h2 class="text-xl font-semibold text-gray-800">Increase Budget</h2>
        </div>

        <div class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Increase Percentage (%)</label>
            <input type="number" id="increasePercentage" value="30" min="0" max="1000"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg input-style"
              onchange="calculateIncrease()"
            >
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Input Budget ($)</label>
            <input type="number" id="increaseInputBudget" value="100"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg input-style"
              onchange="calculateIncrease()"
            >
          </div>

          <div class="bg-gray-50 rounded-lg p-4 space-y-3">
            <div>
              <label class="text-sm text-gray-600">Budget After Increase</label>
              <div id="increasedBudget" class="text-xl font-semibold text-gray-900">$130.00</div>
            </div>
            <div>
              <label class="text-sm text-gray-600">Required Decrease to Return (%)</label>
              <div id="requiredDecrease" class="text-xl font-semibold text-blue-600">23.08%</div>
            </div>
            <div>
              <label class="text-sm text-gray-600">Returning Budget</label>
              <div id="returnedIncreaseBudget" class="text-xl font-semibold text-gray-900">$100.00</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Charts Section -->
    <div class="grid md:grid-cols-2 gap-8 mt-8">
      <div class="gradient-card rounded-2xl shadow-lg p-6">
        <h3 class="text-lg font-semibold text-gray-800 mb-4">Decrease Budget Flow</h3>
        <canvas id="decreaseChart" height="200"></canvas>
      </div>
      <div class="gradient-card rounded-2xl shadow-lg p-6">
        <h3 class="text-lg font-semibold text-gray-800 mb-4">Increase Budget Flow</h3>
        <canvas id="increaseChart" height="200"></canvas>
      </div>
    </div>

    <!-- Footer -->
    <div class="mt-8 text-center text-gray-500 text-sm py-6">
      <p>© 2025 Daily Budget Rules Calculator</p>
    </div>
  </div>

  <script>
    let decreaseChart, increaseChart;

    function initCharts() {
      // Initialize Decrease Chart
      const decreaseCtx = document.getElementById('decreaseChart').getContext('2d');
      decreaseChart = new Chart(decreaseCtx, {
        type: 'line',
        data: {
          labels: ['Original', 'After Decrease', 'After Return'],
          datasets: [{
            label: 'Budget Flow',
            data: [100, 60, 100],
            borderColor: '#ef4444',
            backgroundColor: 'rgba(239, 68, 68, 0.1)',
            fill: true,
            tension: 0.4
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              display: false
            }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });

      // Initialize Increase Chart
      const increaseCtx = document.getElementById('increaseChart').getContext('2d');
      increaseChart = new Chart(increaseCtx, {
        type: 'line',
        data: {
          labels: ['Original', 'After Increase', 'After Return'],
          datasets: [{
            label: 'Budget Flow',
            data: [100, 130, 100],
            borderColor: '#22c55e',
            backgroundColor: 'rgba(34, 197, 94, 0.1)',
            fill: true,
            tension: 0.4
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              display: false
            }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    }

    function formatCurrency(value) {
      return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }

    function formatPercent(value) {
      return new Intl.NumberFormat('en-US', {
        style: 'percent',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value / 100);
    }

    function calculateDecrease() {
      const decreasePercent = parseFloat(document.getElementById('decreasePercentage').value) / 100;
      const inputBudget = parseFloat(document.getElementById('decreaseInputBudget').value);
      
      const decreasedAmount = inputBudget * (1 - decreasePercent);
      const requiredIncreasePercent = (1 / (1 - decreasePercent) - 1) * 100;
      const returnedAmount = decreasedAmount * (1 + requiredIncreasePercent / 100);

      document.getElementById('decreasedBudget').textContent = formatCurrency(decreasedAmount);
      document.getElementById('requiredIncrease').textContent = formatPercent(requiredIncreasePercent);
      document.getElementById('returnedDecreaseBudget').textContent = formatCurrency(returnedAmount);

      // Update chart
      decreaseChart.data.datasets[0].data = [inputBudget, decreasedAmount, returnedAmount];
      decreaseChart.update();
    }

    function calculateIncrease() {
      const increasePercent = parseFloat(document.getElementById('increasePercentage').value) / 100;
      const inputBudget = parseFloat(document.getElementById('increaseInputBudget').value);
      
      const increasedAmount = inputBudget * (1 + increasePercent);
      const requiredDecreasePercent = (1 - 1 / (1 + increasePercent)) * 100;
      const returnedAmount = increasedAmount * (1 - requiredDecreasePercent / 100);

      document.getElementById('increasedBudget').textContent = formatCurrency(increasedAmount);
      document.getElementById('requiredDecrease').textContent = formatPercent(requiredDecreasePercent);
      document.getElementById('returnedIncreaseBudget').textContent = formatCurrency(returnedAmount);

      // Update chart
      increaseChart.data.datasets[0].data = [inputBudget, increasedAmount, returnedAmount];
      increaseChart.update();
    }

    // Initialize
    initCharts();
    calculateDecrease();
    calculateIncrease();
  </script>
</body>
</html>
