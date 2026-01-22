# TASK_5
This task is about process handling in linux.

---

## Process
### What is "Process"?
In Linux, a Process is just a program in action.
- **The Program:** This is the recipe (like a file sitting on your hard drive, e.g., chrome.exe or discord).
- **The Process:** This is the chef actually cooking the meal (the program running in your RAM and using the CPU).

### PID (Process ID) and Parents
Every process has a unique ID number called a PID (Process ID). It’s like a ticket number at a deli. Processes also have a family tree:
- **Parent Process:** A process that starts another process.
- **Child Process:** The process that was started. If you open a Terminal and then type neofetch, the Terminal is the Parent, and neofetch is the Child.

### States and Signals
As a process runs, it moves through different "moods" or States:
- **Running (R):** It’s actively using the CPU right now.
- **Sleeping (S):** It’s waiting for something to happen (like you to click a button).
- **Zombie (Z):** The process is finished, but its "parent" hasn't acknowledged it yet. It’s a ghost in the system!

### Nice Levels (Priority)
Linux processes have a "Niceness" score.
- Nice Value: Ranges from -20 (Highest priority, "I am mean and want all the CPU") to 19 (Lowest priority, "I am nice and will wait my turn").
- You can start a program with a specific priority: ` nice -n 10 backup_script.sh `

### Backgrounding Processes (&)
Sometimes you want to run a command but keep using your terminal.
- The Command: ` sleep ` 100 &
- The ` & `: This tells Linux "Run this in the background."
- You can then use the command ` jobs ` to see all your background tasks.

---

## ps command
### (Process Status)
ps stands for Process Status. It’s like taking a still photo of what’s happening at this exact microsecond.

 If you just type ps, you won't see much. To see the "big picture," most people use these flags:
#### The "All-Star" Command: ` ps aux `
- **a:** Show processes for all users.
- **u:** Show "user-oriented" format (who owns it, how much RAM it uses).
- **x:** Show processes that aren't attached to a terminal (background stuff).

#### What the columns mean:
- **USER:** Who started it.
- **%CPU / %MEM:** How much of your computer's "brain" and "memory" it's hogging.
- **COMMAND:** The actual name of the program.

#### Become a detective of processes
To find a specific needle in the haystack of running processes, we use the "pipe" character ` | `. It takes the output of one command and feeds it into another.

Combined with ` grep ` (which is like a "search" or "find" tool), you become a Linux detective.
- If your computer is acting slow and you think Discord or Steam is the culprit, you don't want to read a list of 500 processes. You do this:
  ` ps aux | grep [app-name] `
  
  example: ` ps aux | grep discord `

  ##### How it works
  1. ` ps aux `: Generates a massive list of everything running.
  2. ` | `: "Pipes" that massive list into the next command.
  3. ` grep discord `: Filters that list and only shows lines containing the word "discord."

---

## top command
### Table of Processes
If ` ps ` is a photo, top is a live security camera feed. It updates every few seconds.

When you run ` top `, the list moves constantly. The hungriest programs (the ones using the most CPU) stay at the top.

### Cool `top` Shortcuts:
While `top` is running, you can hit these keys to control it:
- **M:** Sort by Memory usage (useful if your computer is lagging).
- **P:** Sort by CPU usage (the default).
- **k:** "Kill" a process. It will ask for the PID. This is how you force-quit a frozen app!
- **q:** Quit top and go back to the terminal.

Since you're interested in the advanced side, you should know that pros rarely use the standard `top` anymore. They use `htop` (The Interactive Upgrade).
- If you can, try typing `htop` in your terminal. It’s like `top`, but with colors, mouse support, and bars that show your CPU cores.
- It shows each CPU core individually (so you can see if only one "brain" is working too hard).
- You can use your arrow keys to scroll and the F9 key to kill a process instantly.

## Difference between `ps` and `top`
| Feature | `ps aux` | `top` |
|---------|----------|-------|
| Style |Static Snapshot|Live Update|
| Best for |Searching for one specific PID|Seeing what's making the fans spin loud|
| Vibe |"Show me what happened."|"Show me what's happening now."|

---

## 🐧 Linux Process Termination: `kill` vs `kill -9`

In Linux, "killing" a process doesn't always mean "destroying" it instantly. It actually means sending a **Signal** to the process. Think of signals as text messages sent to the program's brain.

---

## 1. The Regular `kill` (SIGTERM)
**Command:** `kill <PID>`
**Signal Number:** `15`

When you run a standard kill command, you are sending a **SIGTERM** (Signal Terminate).

* **The Vibe:** It's like a polite tap on the shoulder.
* **What happens:** You are telling the program, *"Hey, it's time to close. Please save your work, delete your temporary files, and shut down."*
* **Best Practice:** Always try this first! It prevents data corruption.
* **The Catch:** The program can **ignore** this signal if it’s stuck in a loop or "frozen."

---

## 2. The Forced `kill -9` (SIGKILL)
**Command:** `kill -9 <PID>`
**Signal Number:** `9`

When you use `-9`, you are sending a **SIGKILL**.

* **The Vibe:** It's a "total delete" button.
* **What happens:** This signal doesn't even go to the program; it goes straight to the Linux Kernel (the boss of the OS). The Kernel instantly pulls the plug on the process.
* **The Result:** The program has **zero** time to save files or clean up. It is gone instantly.
* **When to use:** Only use this if the program is completely frozen and a regular `kill` didn't work.



---

## 3. Comparison Table

| Feature | `kill` (SIGTERM) | `kill -9` (SIGKILL) |
| :--- | :--- | :--- |
| **Politeness** | Very Polite | Ruthless |
| **Cleanup** | Allows the app to save data | Kills instantly; no saving |
| **Can be ignored?** | Yes (if the app is frozen) | No (The Kernel handles it) |
| **Safety** | Safe | Risky (potential data loss) |

---

## 4. How to find the PID to kill it
Before you can kill something, you need its "ID Number" (PID).

1.  **Find it:** `ps aux | grep discord`
2.  **Locate the PID:** It's the number in the second column.
3.  **Execute:** `kill 1234` (Try polite first!)
4.  **Confirm:** If it’s still there, `kill -9 1234`.

---

## 5. Pro Tip: `pkill` and `killall`
If you don't want to look up the PID number, you can use the name directly:

* `pkill discord`: Kills the process named "discord".
* `killall chrome`: Kills **every** window and process related to Chrome.
