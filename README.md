# CSB-Snip-it
this is a release only repository, along with a documentation readme which covers, software stack, software architecture documentation, project development logs and documentation along with the application installer in the releases section.

A desktop application built with Electron and React that helps manage and organize code snippets efficiently.  
[Download Latest Release](./releases)  

---

## 📖 Overview
- **Goal:** my goal for this project was to develop a application that i would use. now most developers would pause me here and say "doesn't your text editor have the option to store code snippets". to you i would say correct, except i also want to improve my coding skills, but i also want to finsih this project in a small time frame of around 2-4 months. i wanted to set some rules for this project, because this project is supposed to help me learn how electron works in react for desktop applications, i also wanted to use a language for the backend that ive never used before exposing me to a new language, and finally i wanted to see how ipc's (interprocess communications) worked between typescript and javascript. so instead of only watching youtube courses on specific languages and scrolling through stack overflow for error solutions i would allow the use of AI to assist in the programming.

BUT
**only under this ruleset**
-1: 0 and i mean 0 copy and pasting.

this is because, you are not learning anything from copying and pasting, if i want to implement a solution i will need to copy it by hand, why?
because by copying it i am reading it, i am remebering it and understanding it.

-2: everything i ask it to do, i also want a summary of why it is making these choices to do so.
why? because then i am also understanding the reasoning of its descicion making and i can also catch when it is making a archeticutral discision that is going to come back and bite me down the line.

-3: third and last rule, for every variable it writes or declares i must rename it meaningfully.
why? because through this i am able to understand my program completely as i have named everything meaningfully and this will avoid ambigious naming dminishing code readability. 

---

## 🧠 Thought Process
- to start the project i first needed to breakdown the stack, now going into this we already know two components of  the stack based on my goals, first electron/react from the framwork, secondly typescript for the backend, so we only need to figure out the method of data storage and packaging: so i ended up choosing something ive used before, being a .db (sql database file) for the data storgage, my reasoning being, ive used it before in many other projects and i don't want to add more learning to this already learning rich project. lastly packaging, this was fairly simple choice, electron-builder allows electron apps to be packaged in win, linux and macos so that was the primary choice.

Project research:

next i needed to figure out what i was going to write myself and what i was going to use from the npm library, along with market reasearch on existing snippet managers.

what i was going to code:
- search functionality
- data classes and data methods (no prebuilt query libraries)
- icon and language dependency extensions/render (these methods will take text like "C#" and find the icon with the name "csharp" linked and render the icon for it into the app)
- auto complete search prediction
- data filtering
- state management
- db and state syncing
- some css elements

What libraries i will use:
- codemirror (for displaying code segments and for code input)
- react-select (for search functonality in selection inputs )
- react-tag-input (for displaying linked tags )
- tailwind css ( to ease the styling workload ) 
---  

## 📚 Libraries & Tools
- **Code Editor / Syntax Highlighting:** reasoning* i chose code mirror for this because it is lightweight and used by industry giants like google for their own displayment of text in the dubug tool.  
- **Select Inputs:** reasoning * i used the select inputs because, while i could write the components myself, it wouldn't add anything to the educational purposes of this project, it only offers a time setback working on such trivial components.
- **Database:** reasoning * SQL was the best option for the database as i have used it before and i know the structuring of queries in SQL. 
- **Styling:** reasoning * again ive used tailwind in previous projects, its faster when writing classes than regular css, so for fast development purposes i picked it for the majority of components with some special components done in regular css.
- **Packaging:** reasoning * its the no brainer option it allows distribution across the big 3 operating systems. (besides macos atm because i need a mac to compile it !!linux and windows is available)  

---

## 🗄 Database Design
 i designed the db  to be set up with three tables:
 Snippets, Shortcuts, Language_Dependencies

 the snipets tabled organises the snippets data into snippet objects, shortcuts has a fixed 4 entries, one entry for each tab, which holds a string array of shortcuts for each tab and lastly language/dependencies which contains all the languages that supports syntax highlighting for code mirror and a dependencies coloumn which users can add too for each respected language.
 
it was designed this way for simplicity and seperation of the three types of objects we are using.

some challenges for managing data was allowing the importing of databases, for this we didn't apply a scheme check for this release but it is on the to do for the first patch of the app.
(basically there is no db validation on the importing of the db. so if the db imported doesn't match the expected input there will be unexpected results and some field locking, though the app will not crash)

---

## ⚙️ Tech Stack
- **Frontend:** (React, Tailwind,)  
- **Backend / Electron:** (Electron APIs, Node.js, typescript)  
- **Database:** (SQLite)  
- **Build Tools:** (Vite/Webpack, electron-builder)  

---

## 📌 User Stories (MoSCoW Method)
**Must Have**  
- Users can create and save code snippets  
- Users can search snippets
- Users can use folder like functionality for quick finding snippets
- Users can edit code snippets
- Users can delete code snippets
- Users can import databases
- Users can restore their previous database
- Users code snippets will have Syntax Highlighting based on language
- Users can view their snippets Meta

**Should Have**  
- Users can change the apps theme to Dark

**Could Have**  


**Won’t Have (for now)**  
- Multi-user collaboration  
- Mobile support
- Cloud capabilities
- Encryption
- Login
- Register

---

## 🎨 UI/UX Design Journey


---

## 🛠 Development  

Challenges and solutions I encountered during development:  

- **State management issues:**  
  - *Challenge:* Keeping the snippet editor form in sync when switching between different snippets. Tags, dependencies, and fields sometimes didn’t refresh correctly because React state was holding on to old values.  
  - *Solution:* I restructured components to listen for prop changes and reset their local state whenever a new snippet was selected. This ensured the editor always reflected the latest snippet data.  

- **Electron quirks:**  
  - *Challenge:* Handling file system operations (import/export of databases) in a safe way. At first, the backup and replacement system wrote JSON files instead of proper `.db` files, and paths sometimes conflicted with where Electron stores app data.  
  - *Solution:* I updated the logic to always back up and restore using the correct file extension and confirmed paths via Electron’s `app.getPath("userData")`. This kept user data consistent.  

- **Database handling:**  
  - *Challenge:* At first, the app used plain JSON for persistence, but that created issues with scalability, importing, and overwriting user data. There was also the problem of mismatched schemas when importing a database from outside.  
  - *Solution:* I switched to using a proper SQLite database, set up a consistent schema for snippets, and added error handling for invalid imports (e.g., when tables don’t match). This made the database much more reliable.  

- **Cross-platform testing:**  
  - *Challenge:* Most development happened on Windows, but Electron behaves slightly differently on macOS and Linux (file paths, packaging formats, default locations for the database).  
  - *Solution:* I standardized all file paths through Electron’s APIs instead of hardcoding, and tested builds on multiple platforms through electron-builder. This helped ensure consistent behavior.  

---

## 📦 Release  

- **How I packaged the app (electron-builder installer):**  
  I used **electron-builder** to generate native installers for Windows. Initially, the output folder was massive (around 500 MB) because of bundling unoptimized dependencies. After trimming unnecessary assets and excluding dev-only files, the build size dropped closer to ~100 MB.  

- **Steps I took to test before release:**  
  - Verified installation on a clean Windows environment.  
  - Checked that the installer created the correct shortcuts and persisted the database under the user’s AppData folder.  
  - Imported/exported test databases to ensure backup and restore worked.  
  - Verified that the app could launch from the shortcut without needing to re-run the installer.  

- **Publishing process (GitHub Releases):**  
  I created a separate **public GitHub repository** for distribution. This repo only contains the REA

## 🔮 Future Improvements
- the app atm is 500mb which is massive, i know i can get that size down to 110-130mb by trimming code files im not using, optimisation and also skimming and setting json rules in the app config to only install dependencies and libraries that are required at run time that means anything for development, all the files node doesn't use, etc. will be trimmed when compiled
- zod schema verification, the zod shcema check is written it just didnt make the final because i was unsure on how it works and wanted to have a working release by the end of the 4 month period so i can link it in some job applications
- note taking
- more themes
- i want the application to usable completely by keyboard only


---

## 🙏 Closing Notes

This project has been one of my biggest learning experiences so far. Along the way, I’ve gained practical skills in:  

- **TypeScript:** Writing strongly-typed code that reduced bugs and made refactoring safer.  
- **Electron:** Understanding how the main and renderer processes communicate, and how to package cross-platform desktop apps.  
- **IPC (Inter-Process Communication):** Setting up secure channels between the frontend and backend logic of the app.  
- **State Management in React:** Handling complex form state (snippets, tags, dependencies) and ensuring components stay in sync.  
- **Database Design & Handling:** Moving from simple JSON storage to a structured SQLite database with schema validation and import/export support.  
- **Vite:** Using a modern build tool for fast hot-reloads and bundling, which sped up development compared to older setups.  
- **UI/UX Iteration:** Starting with a basic interface and gradually improving it based on real usage and testing.  

Overall, this project helped me bridge the gap between frontend and backend concepts, and gave me hands-on experience building and releasing a full desktop application from scratch.  
