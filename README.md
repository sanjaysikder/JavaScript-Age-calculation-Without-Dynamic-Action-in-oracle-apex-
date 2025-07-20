# Oracle APEX: Dynamic Age Calculator using JavaScript

This project demonstrates how to calculate and display age (`Years`, `Months`, `Days`) from a selected Date of Birth (`DOB`) in Oracle APEX using **pure JavaScript**—without creating any Dynamic Actions in the Page Designer.

---

## 🔧 Features

- Automatically calculates age when the user selects a Date of Birth.
- Updates a display-only APEX item (e.g., `P1_AGE`) with formatted text like `25 Years 3 Months 10 Days`.
- No dynamic actions required—just JavaScript in `Function and Global Variable Declaration`.

---

## 🧱 Required APEX Page Items

| Page Item | Type        | Description                    |
|-----------|-------------|--------------------------------|
| `P1_DOB`  | Date Picker | User input for Date of Birth   |
| `P1_AGE`  | Display Only| Output item showing full age   |

---

## 📝 Step-by-Step Implementation

### 1. Open Your APEX Page

Go to **Page Designer** → Select the appropriate **Page**.

---

### 2. Define Items

Ensure the following items exist on the page:

- `P1_DOB`: A **Date Picker** item for selecting DOB.
- `P1_AGE`: A **Display Only** item to show calculated age.

---

### 3. Add JavaScript to Function and Global Variable Declaration

In Page Designer:

- Click on the **Page** (top left in Page Designer).
- Scroll down to the **"Function and Global Variable Declaration"** section.
- Paste the following JavaScript code:

```javascript
function calculateAge(dobStr) {
    if (!dobStr) return '';

    const dob = new Date(dobStr);
    const today = new Date();

    let years = today.getFullYear() - dob.getFullYear();
    let months = today.getMonth() - dob.getMonth();
    let days = today.getDate() - dob.getDate();

    if (days < 0) {
        months--;
        const prevMonth = new Date(today.getFullYear(), today.getMonth(), 0);
        days += prevMonth.getDate();
    }

    if (months < 0) {
        years--;
        months += 12;
    }

    return `${years} Years ${months} Months ${days} Days`;
}

// Attach event handler to P1_DOB item after APEX is ready
apex.jQuery(function($) {
    $('#P1_DOB').on('change', function() {
        var dob = $v('P1_DOB');
        var age = calculateAge(dob);
        $s('P1_AGE', age);
    });
});
