Creation Date: 2025-07-01
2025-07-01T10:34:10-04:00

# Notes:
Add to all projects 
- yaml  
status: active priority: high due: 2025-06-30 tags: project  
- [status:: high] [priority:: high] [due:: 2025-06-30]

### List 5 most recent "projects"
- LIST  
FROM ""  
SORT file.mtime DESC  
LIMIT 5

### Table of projects with due date
- TABLE status, priority, due  
FROM #project  
SORT due ASC

### Show as calendar
- CALENDAR due  
FROM #project  
WHERE due

### Progress bar
- const tasks = dv.page("Test").file.tasks  
let completedTasks = tasks.where(t => t.completed)  
dv.span("![progress](https://progress-bar.dev/" + parseInt((completedTasks.length / tasks.length) * 100) + "/)")

## Tags: #dataview #plugin 

## Reference:
[https://www.xda-developers.com/used-obsidians-dataview-plugin-live-dashboard/] 


