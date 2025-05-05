
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Cycle Nutrition Tracker for Indian Women</title>
<style>
  /* Reset and base */
  * {
    box-sizing: border-box;
  }
  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #FFF1E6, #FFDDD2);
    color: #5A3E36;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 600px;
    max-width: 350px;
    margin-left: auto;
    margin-right: auto;
    padding: 1rem 1rem 3rem 1rem;
    user-select: none;
  }

  h1 {
    font-weight: 700;
    margin-bottom: 0.25rem;
    color: #9C3D3D;
    text-align: center;
    font-size: 1.8rem;
  }
  h2 {
    font-weight: 600;
    margin-top: 0.5rem;
    color: #6D4C41;
  }
  h3 {
    font-weight: 600;
    margin-top: 0.5rem;
    color: #7C5B51;
  }

  .container {
    background: #fff;
    box-shadow: 0 4px 8px rgba(156,61,61,0.3);
    border-radius: 16px;
    padding: 1rem 1.5rem 1.5rem 1.5rem;
    width: 100%;
    max-width: 350px;
  }
  label {
    font-weight: 600;
    display: block;
    margin-top: 1rem;
    margin-bottom: 0.25rem;
    color: #8B4D4D;
  }
  input, select, button {
    width: 100%;
    padding: 0.5rem 0.6rem;
    border-radius: 8px;
    border: 2px solid #D99B85;
    font-size: 1rem;
    font-weight: 600;
    color: #6D4C41;
    background: #FFF7F2;
    transition: border-color 0.3s ease;
  }
  input[type="date"]::-webkit-calendar-picker-indicator {
    filter: invert(35%) sepia(72%) saturate(356%) hue-rotate(340deg) brightness(92%) contrast(85%);
  }
  input:focus, select:focus {
    outline: none;
    border-color: #9C3D3D;
  }
  button {
    margin-top: 1.5rem;
    background: #9C3D3D;
    color: #FFF;
    font-weight: 700;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 4px 10px rgba(156,61,61,0.7);
  }
  button:hover {
    background: #7C2C2C;
  }

  .result {
    margin-top: 2rem;
    text-align: center;
  }
  .phase {
    font-size: 1.5rem;
    font-weight: 700;
    color: #B85151;
    margin-bottom: 0.5rem;
  }
  .foods-list {
    list-style: none;
    padding: 0;
    margin: 0.5rem 0 1rem 0;
    font-weight: 600;
    color: #6E4A3E;
  }
  .foods-list li {
    margin: 0.3rem 0;
  }
  .quote {
    font-style: italic;
    margin-top: 1rem;
    color: #8B5E3C;
    font-weight: 600;
  }

  /* Footer note */
  .footer-note {
    margin-top: auto;
    font-size: 0.75rem;
    color: #A37B71;
    user-select: text;
  }

  /* Responsive for mobile */
  @media (max-width: 360px) {
    body {
      padding: 1rem 0.5rem 3rem 0.5rem;
    }
  }
</style>
</head>
<body>
  <h1>Cycle Nutrition Tracker 🇮🇳</h1>
  <div class="container">
    <form id="cycleForm" autocomplete="off" aria-label="Cycle input form for tracking and suggestions">
      <label for="startDate">Start Date of Last Period</label>
      <input type="date" id="startDate" name="startDate" required aria-required="true" aria-describedby="startDateHelp" max="" />
      <small id="startDateHelp" style="color:#A37B71; font-size:0.75rem;">Select the first day of your last menstrual cycle</small>

      <label for="cycleLength">Cycle Length (days)</label>
      <select id="cycleLength" name="cycleLength" required aria-required="true" aria-describedby="cycleLengthHelp" >
        <option value="" disabled selected>Select cycle length</option>
        <option value="21">21 days (Short)</option>
        <option value="24">24 days</option>
        <option value="28">28 days (Average)</option>
        <option value="30">30 days</option>
        <option value="32">32 days</option>
        <option value="35">35 days (Long)</option>
      </select>
      <small id="cycleLengthHelp" style="color:#A37B71; font-size:0.75rem;">Choose your average menstrual cycle length</small>

      <button type="submit" aria-describedby="submitHelp">Track My Cycle</button>
      <small id="submitHelp" style="color:#A37B71; font-size:0.75rem;">Get food & motivation suggestions based on your current cycle phase</small>
    </form>
    <section id="result" class="result" aria-live="polite" aria-atomic="true" tabindex="-1"></section>
  </div>
  <div class="footer-note">Designed especially for Indian women. Encouraging wellness and positivity.</div>

<script>
(function() {
  const form = document.getElementById('cycleForm');
  const startDateInput = document.getElementById('startDate');
  const resultContainer = document.getElementById('result');
  const cycleLengthSelect = document.getElementById('cycleLength');

  // Set max date for startDateInput as today
  const todayStr = new Date().toISOString().split('T')[0];
  startDateInput.setAttribute('max', todayStr);

  // Indian Food Suggestions by phase
  const foodSuggestions = {
    menstrual: {
      foods: [
        "Palak (Spinach) - Iron-rich",
        "Moong dal (Green gram lentils) - Protein",
        "Turmeric milk (Haldi doodh)",
        "Papaya - Helps regulate cycle",
        "Dates - Energy & iron boost",
        "Carrot - Rich in Vitamin A"
      ],
      quote: "Your strength blooms even in the storm. Embrace this phase with care."
    },
    follicular: {
      foods: [
        "Mango - Rich in vitamins",
        "Cucumber - Hydrating & cooling",
        "Beetroot - Blood cleanser",
        "Chickpeas (Chana) - Protein & fiber",
        "Orange - Vitamin C boost",
        "Amaranth (Rajgira) - Nutrients for energy"
      ],
      quote: "New beginnings start today. Harness your growing energy with positivity."
    },
    ovulation: {
      foods: [
        "Pomegranate - Helps fertility",
        "Almonds and walnuts - Healthy fats",
        "Ash gourd (Petha) - Cleansing and hydrating",
        "Tomatoes - Rich in antioxidants",
        "Amla (Indian gooseberry) - Immunity booster",
        "Sweet potatoes - Nutrient-dense carb"
      ],
      quote: "You shine brightest now! Let your confidence light the way."
    },
    luteal: {
      foods: [
        "Banana - Mood stabilizer",
        "Fenugreek (Methi) - Hormonal balance",
        "Bottle gourd (Lauki) - Cooling & light food",
        "Ginger - Eases discomfort",
        "Turmeric - Anti-inflammatory",
        "Yogurt (Dahi) - Good gut bacteria"
      ],
      quote: "Stay calm and grounded. Nourish your body and mind with kindness."
    }
  };

  // Calculate the difference in days between two dates
  function diffDays(date1, date2) {
    const diffTime = date2.getTime() - date1.getTime();
    return Math.floor(diffTime / (1000 * 60 * 60 * 24));
  }

  // Determine cycle phase given day in cycle and cycle length
  function getCyclePhase(day, length) {
    const menstrualDays = Math.min(5, Math.floor(length * 0.18));
    const follicularDays = Math.min(8, Math.floor(length * 0.32));
    const ovulationDays = Math.min(3, Math.floor(length * 0.10));

    if (day < 1) return "menstrual";
    if (day <= menstrualDays) return "menstrual";
    if (day <= menstrualDays + follicularDays) return "follicular";
    if (day <= menstrualDays + follicularDays + ovulationDays) return "ovulation";
    if (day <= length) return "luteal";
    return "menstrual";
  }

  // Format date nicely in Indian style dd-mm-yyyy
  function formatDateInd(date) {
    return date.toLocaleDateString('en-IN', {
      day: 'numeric',
      month: 'short',
      year: 'numeric'
    });
  }

  // Generate HTML for food list
  function buildFoodList(foods) {
    return '<ul class="foods-list">' + foods.map(f => '<li>' + f + '</li>').join('') + '</ul>';
  }

  // Main handler for form submit
  form.addEventListener('submit', function(e) {
    e.preventDefault();

    const startDateStr = startDateInput.value;
    const cycleLength = parseInt(cycleLengthSelect.value, 10);
    if (!startDateStr || !cycleLength) {
      resultContainer.innerHTML = '<p style="color:#B03E3E; font-weight:700;">Please select both start date and cycle length.</p>';
      resultContainer.focus();
      return;
    }

    const startDate = new Date(startDateStr);
    const today = new Date();
    const dayInCycle = diffDays(startDate, today) + 1;
    if (dayInCycle < 1) {
      resultContainer.innerHTML = '<p style="color:#B03E3E; font-weight:700;">Start date is in the future. Please select a valid date.</p>';
      resultContainer.focus();
      return;
    }

    let phase = getCyclePhase(dayInCycle, cycleLength);
    const phaseData = foodSuggestions[phase];

    let html = '';
    html += '<div class="phase" aria-label="Current cycle phase">Phase: ' + phase.charAt(0).toUpperCase() + phase.slice(1) + '</div>';
    html += '<h3>Suggested Indian Foods to Eat:</h3>';
    html += buildFoodList(phaseData.foods);
    html += '<div class="quote" aria-label="Motivational quote">' + phaseData.quote + '</div>';
    html += '<small style="color:#A37B71; font-size:0.75rem; display:block; margin-top:1rem;">Day ' + dayInCycle + ' of your ' + cycleLength + '-day cycle.<br>Last period start: ' + formatDateInd(startDate) + '</small>';

    resultContainer.innerHTML = html;

    // Scroll smoothly if supported, fallback to instant scroll
    if ('scrollBehavior' in document.documentElement.style) {
      resultContainer.scrollIntoView({ behavior: 'smooth' });
    } else {
      resultContainer.scrollIntoView();
    }

    // Move keyboard focus to result container for accessibility
    resultContainer.focus();
  });
})();
</script>
</body>
</html>
