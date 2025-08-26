# Drill 2 — Bash Scripting and Commands

This drill covers safe Bash scripting, working with pipes, processes, ports, software management, and miscellaneous commands.

---

```bash
#!/usr/bin/env bash
# Drill 2 

set -euo pipefail  
# Safe bash scripting (stop on error, undefined variable, or failed pipe)

###############################################
# PIPES
###############################################

# 1. Download the contents of the book
# wget : download files from internet
# -O goblet.txt : save the file as goblet.txt
wget https://www.gutenberg.org/files/35359/35359.txt -O goblet.txt

# 2. Print the first three lines in the book
# head -n 3 : show only the first 3 lines
head -n 3 goblet.txt

# 3. Print the last 10 lines in the book
# tail -n 10 : show only the last 10 lines
tail -n 10 goblet.txt

# 4. Count occurrences of specific words
# grep -o : print each match on a new line
# wc -l   : count how many lines (number of matches)
echo "Harry: $(grep -o 'Harry' goblet.txt | wc -l)"
echo "Ron: $(grep -o 'Ron' goblet.txt | wc -l)"
echo "Hermione: $(grep -o 'Hermione' goblet.txt | wc -l)"
echo "Dumbledore: $(grep -o 'Dumbledore' goblet.txt | wc -l)"

# 5. Print lines 100 through 200
# sed -n '100,200p' : print only lines 100 to 200
sed -n '100,200p' goblet.txt

# 6. Count unique words in the book
# tr -cs 'a-zA-Z' '\n' : split into words
# tr 'A-Z' 'a-z' : convert to lowercase
# sort | uniq | wc -l : count unique words
cat goblet.txt | tr -cs 'a-zA-Z' '\n' | tr 'A-Z' 'a-z' | sort | uniq | wc -l


###############################################
# PROCESSES, PORTS
###############################################

# 7. List your browser’s process ids (pid) and parent process ids (ppid)
# Replace 'chrome' with 'firefox' if you use Firefox
ps -C chrome -o pid,ppid,cmd

# 8. Stop the browser application from the command line
# pkill : kill processes by name
pkill chrome

# 9. List the top 3 processes by CPU usage
# ps -eo : list processes with info
# --sort=-%cpu : sort by CPU usage (descending)
# head -n 4 : top 3 + header
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -n 4

# 10. List the top 3 processes by memory usage
# --sort=-%mem : sort by memory usage (descending)
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -n 4

# 11. Start a Python HTTP server on port 8000
# python3 -m http.server : run built-in Python web server
# 8000 : port number
python3 -m http.server 8000  
# run in background

# 12. Stop the Python HTTP server started above
lsof -i :8000
kill 5179

# 13. Start a Python HTTP server on port 90
sudo python3 -m http.server 90

# 14. Display all active connections and TCP/UDP ports
# netstat -tulnp : list ports and connections
netstat -tulnp

# 15. Find the PID of process listening on port 5432
# lsof : list open files (including network sockets)
lsof -i :5432 |


###############################################
# MANAGING SOFTWARE
###############################################

# 16. Install htop, vim, and nginx (Ubuntu)
sudo apt update
sudo apt install -y htop vim nginx

# 17. Uninstall nginx
sudo apt remove -y nginx


###############################################
# MISC
###############################################

# 18. What's your local IP address?
hostname -I

# 19. Find the IP address of google.com
ping -c 1 google.com

# 20. Check if internet is working
# ping exit code 0 = success
ping -c 3 8.8.8.8 

# 21. Where is the node command located? What about code?
which node
which code
