# ⚡ Linux CPU Spikes: Understanding & Responding

When your CPU usage hits **100%**, your computer's "brain" is fully occupied. This guide explains what happens under the hood and how to take control.

---

## 1. What Happens Inside the CPU?

### The "Waiting Room" (Load Average)
When the CPU is at capacity, new tasks enter a **Queue**. 
* **Context Switching:** The CPU "juggles" tasks, working on Process A for a millisecond, then switching to Process B.
* **The Result:** You experience **Lag**. Your mouse stutters or typing is delayed because the CPU is too busy juggling to process your inputs.



### Thermal Throttling
100% usage creates intense heat.
* **The Fan:** Your system ramps up fan speeds to cool down.
* **Throttling:** If the heat gets too high, the CPU **purposely slows itself down** to prevent melting. This makes the lag even worse because your processor is now moving in "slow motion."

---

## 2. Investigating the Culprit

When the system lags, you must identify the "Resource Hog."

### A. The Live View (`top` or `htop`)
* Look at the **%CPU** column. The process at the top is the primary user.
* Check the **Load Average** (top right in `top`).
    * These numbers represent load over **1, 5, and 15 minutes**.
    * If these numbers are higher than your number of CPU cores, your system is bottlenecked.



---

## 3. How to Respond (Three Levels of Intervention)

If a process is spiking and won't stop, use these commands:

| Level | Command | What it does |
| :--- | :--- | :--- |
| **1. Be Nice** | `sudo renice +10 -p [PID]` | Lowers the priority so other apps can move to the front of the line. |
| **2. Polite Quit** | `kill [PID]` | Sends a **SIGTERM**; asks the app to save and close. |
| **3. Force Stop** | `kill -9 [PID]` | Sends a **SIGKILL**; instantly pulls the plug on the process. |

---

## 4. Special Case: Daemon Spikes
Sometimes a background worker (Daemon) is the cause (e.g., a file indexer or update manager).

* **Check Status:** `systemctl status [service_name]`
* **Restart:** `sudo systemctl restart [service_name]`
* **Temporary Stop:** `sudo systemctl stop [service_name]`



---

## 5. Summary Checklist

* **Symptom: Input Lag** → CPU Queue is too long. Use `htop` to find the hog.
* **Symptom: Loud Fans** → High heat. Check `sensors` or reduce load.
* **Symptom: Total Freeze** → System Exhausted. Use `Alt+SysRq+REISUB` for a safe emergency reboot.
