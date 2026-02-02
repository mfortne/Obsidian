Tags: #Obsidian #plugin #templater 

---
created: "<% tp.file.creation_date("YYYY-MM-DD") %>"
year: <% tp.file.creation_date("YYYY") %>
topic: <% tp.file.cursor(1) %>
tags:
  - 
aliases: []
---
<%*
//If File is untitled prompt the User to set a Title (Source: https://github.com/SilentVoid13/Templater/discussions/259)
let title = tp.file.title
if (title.startsWith("Untitled")) {
    title = await tp.system.prompt("Title") ?? "Untitled";
    await tp.file.rename(`${title}`);
} %>
Last Modified: `=dateformat(this.file.mtime, "DDDD, HH:mm")`

# <% tp.file.title %> 

<% tp.file.cursor(2) %>

- ` ```dataview List FROM #Important ```
- https://www.macdrifter.com/2021/08/obsidian-templater-fun.html
	- ![[Pasted image 20250614165759.png]]
- https://cassidoo.co/post/obsidian-templater/