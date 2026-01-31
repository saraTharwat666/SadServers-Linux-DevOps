# 🛠️ SadServers Challenge: "Tokamachi" - The Choked Pipe

## 📋 Scenario Overview
A process is reading from a named pipe (`/home/admin/namedpipe`), but the communication stops after a while. The goal was to fix the "blocking" issue between the writer and the slow reader.

## 🔍 Root Cause Analysis
- **Producer-Consumer Mismatch:** The writer was sending messages constantly without delay, while the reader had a `sleep 2` delay between reads.
- **Pipe Saturation:** Since named pipes have a limited buffer, the writer became "Blocked" once the buffer was full, waiting for the slow reader to clear space.

## 🛠️ Solution
Synchronized the writer's speed with the reader's pace by adding a delay to the writer loop.

1. **Terminate existing stuck processes:**
   `pkill -f "while true"`
2. **Run the writer with a matching delay:**
   ```bash
   /bin/bash -c 'while true; do echo "test message" > /home/admin/namedpipe; sleep 2; done' &
