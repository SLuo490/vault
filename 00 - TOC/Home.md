---
banner: "https://motionbgs.com/media/2404/house-on-island-spirited-away.jpg"
banner_y: 0.56629
cssclasses:
  - hide_properties
  - wide-page
obsidianUIMode: preview
---
# 🏠 Home Page

<br>

```dataviewjs
// Create a container div for the clock widget
const clockDiv = this.container.createDiv({ cls: "clock-widget" });

// Inject HTML structure after the container is created
clockDiv.innerHTML = `
  <div style="text-align: center;">
      <h1 id="clock-time" style="font-size: 3em; margin: 0;">Loading...</h1>
      <p id="clock-date" style="margin: 0; color: grey;">Loading...</p>
  </div>
`;

// Cache the clock and date elements outside the update function for performance
const clockElement = clockDiv.querySelector("#clock-time");
const dateElement = clockDiv.querySelector("#clock-date");

// JavaScript function to update the clock and date
function updateClock() {
  const now = new Date();
  
  // Specify options to include AM/PM in the time format
  let timeString = now.toLocaleTimeString([], {
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12: true // This ensures AM/PM is included
  });
  
  // Force AM/PM to be uppercase
  timeString = timeString.toUpperCase();

  // Manually format the date as DD/MM/YYYY
  const day = String(now.getDate()).padStart(2, '0'); // Ensure two digits
  const month = String(now.getMonth() + 1).padStart(2, '0'); // Months are zero-indexed
  const year = now.getFullYear();
  const dateString = `${month}/${day}/${year}`; // Format the date
  
  // Update the content of the clock and date directly
  if (clockElement) {
    clockElement.textContent = timeString;
  }
  if (dateElement) {
    dateElement.textContent = dateString;
  }

  // Schedule the next update
  requestAnimationFrame(updateClock);
}

// Start updating the clock
updateClock();
```

<br>

> [!multi-column]
>
>> [!note]+ Recent
>> ```dataview
>> LIST FROM "" SORT file.mday DESC LIMIT 5
>> ```
>
>> [!warning]+ Todo
>> ```dataviewjs
>> // --- Config ---
>> // Set the path to your daily notes folder (include trailing slash)
>> const dailyNoteFolder = "03 - Daily Note/"; 
>> // --- End Config ---
>>
>> // Construct the expected filename for today
>> const todayFilename = dv.date('today').toFormat("yyyy-MM-dd");
>> const fullPath = dailyNoteFolder + todayFilename;
>>
>> // Optional: Uncomment the next line to show the path being checked
>> // dv.paragraph("Checking for tasks in: " + fullPath); 
>>
>> // Get all tasks from today's daily note file
>> const allTasks = dv.pages('"' + fullPath + '"').file.tasks;
>>
>> // Filter for incomplete tasks first (handle case where file/tasks might not exist)
>> const incompleteTasks = allTasks ? allTasks.where(t => !t.completed) : [];
>>
>> // NOW, check if the filtered list (incompleteTasks) has any items
>> if (incompleteTasks.length > 0) {
>>    // If yes, display the list of incomplete tasks
>>    dv.taskList(incompleteTasks, false); 
>> } else {
>>    // If the filtered list is empty, display the custom message
>>    dv.paragraph("No incomplete tasks found for today."); 
>> }
>> ```
>

<br>

```dataviewjs
dv.span("Obsidain Activity")

// 1. Configuration for the Calendar
const calendarData = {
    // year: 2025, // Optional: Uncomment and set a specific year. Defaults to the current year.
    intensityScaleStart: 1, // Optional: Set a minimum intensity if desired
    intensityScaleEnd: 10, // Optional: Set a maximum intensity cap if desired
    entries: [] // This will be populated below
};

// Define folders to exclude (add more paths as needed)
const excludedFolders = ["Templates/", ".obsidian/"]; 
// Check if a file path starts with any of the excluded folder paths
const isExcluded = (path) => excludedFolders.some(folder => path.startsWith(folder));

// 2. Data Gathering and Processing
// Get all pages in the vault. Use "" for the whole vault.
// You can filter further here if needed (e.g., add tags, specific folders)
const pages = dv.pages('""')
    .where(p => !isExcluded(p.file.path)); // Apply exclusion filter

// Group pages by their modification date (formatted as YYYY-MM-DD)
const pagesGroupedByModDate = pages.groupBy(p => p.file.mday.toISODate());

// 3. Populate Calendar Entries
for (let group of pagesGroupedByModDate) {
    // group.key is the date string (e.g., "2025-04-18")
    // group.rows is an array of page objects modified on that date
    calendarData.entries.push({
        date: group.key,                   // The modification date
        intensity: group.rows.length,      // Use the count of modified files as intensity
    });
}

// 4. Render the Calendar
renderHeatmapCalendar(this.container, calendarData);
```

