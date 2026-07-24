1. Evaluation of Recovery Mechanisms in vi
Swap Files (.swp):

Mechanism: As you edit, vi creates a hidden swap file (e.g., .filename.swp) that records changes made to the buffer, storing unsaved modifications outside volatile memory.

Reliability: High. It captures unsaved changes made up to the moment of the crash, independent of manual save states.

Undo History (u / U):

Mechanism: Tracks changes within the active editing session temporarily in system RAM.

Reliability: None after a crash. Because the RAM contents and internal session state are wiped completely during a system crash, undo history cannot be recovered.

Registers:

Mechanism: Temporary storage buffers used for copy, cut, and paste operations within vi.

Reliability: None after a crash. Like undo history, registers exist purely in volatile system memory and do not survive a sudden power loss or kernel panic.

Backup Files (~ files):

Mechanism: Depending on configuration settings (such as backup), vi can write a backup copy of the original file prior to overwriting it.

Reliability: Moderate for the last saved state. It preserves the file as it looked when last explicitly written to disk, but it will not contain the unsaved edits made during the crashed session.

Auto-Recovery (vi -r):

Mechanism: A built-in command-line recovery utility that scans for active swap files and extracts the text modifications stored inside them.

Reliability: High. When paired with swap files, it provides an automated pathway to rebuild the modified text buffer safely.

2. Proposed Most Reliable Recovery Strategy & Justification
The Strategy:

Open the terminal and run vi -r filename (or simply open the original filename, which will automatically detect the active swap file and prompt a recovery warning).

Inspect the recovered text buffer to ensure data integrity and completeness.

Save the recovered text permanently using :w filename (or a new filename), and clean up the leftover swap file from the directory.

Justification:
Swap files paired with auto-recovery (vi -r) represent the only mechanism that records incremental state changes onto non-volatile storage during an active editing session. Since undo history and registers vanish completely with a power failure, and traditional backup files only capture the pre-session state, the swap file is the sole artifact capable of rescuing unsaved work post-crash.
