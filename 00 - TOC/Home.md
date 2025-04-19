---
banner: "https://motionbgs.com/media/2404/house-on-island-spirited-away.jpg"
banner_y: 0.58629
cssclasses:
  - hide_properties
  - wide-page
obsidianUIMode: preview
---
```dataviewjs
// --- Configuration ---
const headingText = "🏠 Home Page";
// Using a div but styling it like a heading
const headingFontSize = "2em"; // Adjust to match desired heading size (e.g., h1 size)
const headingFontWeight = "bold"; // Or "normal", "600", etc.
// --- End Configuration ---

// 1. Create the main container with Flexbox
const mainContainer = this.container.createDiv({
    cls: "heading-clock-container",
    attr: {
        style: `
            display: flex; 
            align-items: center; /* Vertically center the items' boxes */
            justify-content: space-between; /* Push heading left, clock right */
            margin-bottom: 1em; 
            margin-top: 1em;
            flex-wrap: wrap; 
            position: relative; /* Add this for z-index context */
            z-index: 10;        /* Add this to ensure it's above banner */
        `
    }
});

// 2. Create the Heading element ** AS A DIV ** and apply styles
mainContainer.createEl('div', { // *** Using 'div' ***
    text: headingText,
    attr: { 
        style: `
            margin: 0; /* Reset margin */
            padding: 0; /* Reset padding */
            font-size: ${headingFontSize}; 
            font-weight: ${headingFontWeight};
            line-height: 1.2; /* Adjust if necessary */
        ` 
    }
});

// 3. Create the Clock container element
const clockContainer = mainContainer.createDiv({
    cls: "clock-widget",
    attr: { 
        style: "text-align: right;" 
    }
});

// 4. Inject Clock's HTML Structure 
clockContainer.innerHTML = `
    <div id="clock-time" style="font-size: 1.4em; margin: 0; line-height: 1;">Loading...</div>
    <div id="clock-date" style="margin: 0; color: white; font-size: 0.85em; line-height: 1;">Loading...</div>
`; 

// 5. Cache Clock Elements
const clockTimeElement = clockContainer.querySelector("#clock-time");
const clockDateElement = clockContainer.querySelector("#clock-date");

// 6. Update Clock Function 
function updateClock() {
    const now = new Date();
    //Include seconds in time
    let timeString = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: true }).toUpperCase();
    const day = String(now.getDate()).padStart(2, '0');
    const month = String(now.getMonth() + 1).padStart(2, '0'); 
    const year = now.getFullYear();
    const dateString = `${month}/${day}/${year}`;
    if (clockTimeElement) clockTimeElement.textContent = timeString;
    if (clockDateElement) clockDateElement.textContent = dateString;
    requestAnimationFrame(updateClock);
}

// 7. Start the clock
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

