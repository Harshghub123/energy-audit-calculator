<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Energy Audit Calculator</title>
    <style>
        /* Base styles */
        body {
            font-family: Arial, sans-serif;
            background-color: #ecf39e;
            color: #132a13;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background-color: #f7f7f7;
            border-radius: 10px;
            padding: 20px;
            max-width: 600px;
            width: 100%;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        header {
            text-align: center;
            margin-bottom: 20px;
        }

        h1 {
            color: #132a13;
        }

        button {
            background-color: #90a955;
            color: #fff;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }

        button:hover {
            background-color: #132a13;
        }

        input[type="text"],
        input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            margin-bottom: 10px;
            border-radius: 5px;
            border: 1px solid #132a13;
        }

        .appliance-group {
            background-color: #ecf39e;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 15px;
        }

        .hidden {
            display: none;
        }

        #results {
            background-color: #ecf39e;
            padding: 10px;
            border-radius: 5px;
            margin-top: 15px;
        }
    </style>
</head>

<body>
    <div class="container">
        <header>
            <h1>Energy Audit Calculator</h1>
        </header>
        <section class="appliance-section">
            <h2>Enter Appliance Details</h2>
            <form id="appliance-form">
                <div id="appliance-inputs">
                    <!-- Appliance input fields will be dynamically added here -->
                </div>
                <button type="button" id="add-appliance">Add Appliance</button>
                <button type="submit">Calculate</button>
            </form>
        </section>
        <section id="results-section" class="hidden">
            <h2>Results</h2>
            <div id="results"></div>
            <button id="reset">Reset</button>
        </section>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const applianceForm = document.getElementById('appliance-form');
            const applianceInputs = document.getElementById('appliance-inputs');
            const resultsSection = document.getElementById('results-section');
            const resultsDiv = document.getElementById('results');
            const addApplianceButton = document.getElementById('add-appliance');
            const resetButton = document.getElementById('reset');

            let applianceCount = 0;

            // Function to create a new appliance input group
            function createApplianceInputs() {
                applianceCount++;

                const applianceGroup = document.createElement('div');
                applianceGroup.classList.add('appliance-group');
                applianceGroup.id = `appliance-group-${applianceCount}`;

                applianceGroup.innerHTML = `
            <h3>Appliance ${applianceCount}</h3>
            <label for="appliance-name-${applianceCount}">Name:</label>
            <input type="text" id="appliance-name-${applianceCount}" required>
            <label for="appliance-wattage-${applianceCount}">Wattage (W):</label>
            <input type="number" id="appliance-wattage-${applianceCount}" min="1" required>
            <label for="appliance-voltage-${applianceCount}">Voltage (V):</label>
            <input type="number" id="appliance-voltage-${applianceCount}" min="1" required>
            <label for="appliance-hours-${applianceCount}">Hours per Day:</label>
            <input type="number" id="appliance-hours-${applianceCount}" min="1" required>
            <label for="appliance-days-${applianceCount}">Days per Month:</label>
            <input type="number" id="appliance-days-${applianceCount}" min="1" max="31" required>
            <button type="button" class="remove-appliance" data-id="${applianceCount}">Remove Appliance</button>
        `;

                applianceInputs.appendChild(applianceGroup);

                // Add event listener for removing appliance
                applianceGroup.querySelector('.remove-appliance').addEventListener('click', function (e) {
                    const id = e.target.getAttribute('data-id');
                    document.getElementById(`appliance-group-${id}`).remove();
                });
            }

            // Add the first appliance input group
            createApplianceInputs();

            // Event listener for adding new appliances
            addApplianceButton.addEventListener('click', function () {
                createApplianceInputs();
            });

            // Event listener for calculating energy usage
            applianceForm.addEventListener('submit', function (e) {
                e.preventDefault();

                let totalEnergy = 0;
                let totalCost = 0;
                const costPerKWh = 0.1; // Example cost per kWh

                for (let i = 1; i <= applianceCount; i++) {
                    const name = document.getElementById(`appliance-name-${i}`);
                    const wattage = document.getElementById(`appliance-wattage-${i}`);
                    const voltage = document.getElementById(`appliance-voltage-${i}`);
                    const hours = document.getElementById(`appliance-hours-${i}`);
                    const days = document.getElementById(`appliance-days-${i}`);

                    if (name && wattage && voltage && hours && days) {
                        const energyUsed = (wattage.value * hours.value * days.value) / 1000;
                        totalEnergy += energyUsed;
                        totalCost += energyUsed * costPerKWh;
                    }
                }

                resultsDiv.innerHTML = `
            <p>Total Energy Used: ${totalEnergy.toFixed(2)} kWh</p>
            <p>Estimated Cost: $${totalCost.toFixed(2)}</p>
        `;
                resultsSection.classList.remove('hidden');
            });

            // Event listener for resetting the form
            resetButton.addEventListener('click', function () {
                applianceInputs.innerHTML = '';
                resultsDiv.innerHTML = '';
                resultsSection.classList.add('hidden');
                createApplianceInputs();
            });
        });
    </script>
</body>

</html>

