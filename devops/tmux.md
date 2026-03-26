# TMUX — Terminal Multiplexer

## Sessions

### Creating Sessions
```
tmux new
```
- Open a new session

```
tmux new -s mysession
```
- Open a new **named** session

### Managing Sessions
```
tmux ls
```
- List all sessions

```
tmux attach
```
- Attach to the most recent session

```
tmux attach -t mysession
```
- Attach to a specific session

---

## Prefix (Leader Key)
- **Ctrl + b** — default prefix for all tmux commands

---

## Session Commands (Inside tmux)
- `prefix + d` — detach from session  
- `prefix + s` — show all sessions  
- `prefix + :` — open command prompt  
  - `rename-session` — rename current session  
  - `rename-window` — rename current window  

---

## Windows
- `prefix + c` — create new window  
- `prefix + x` — kill current window  
- `prefix + ,` — rename window  

---

## Panes
- `prefix + %` — split vertically  
- `prefix + "` — split horizontally  
- `prefix + arrow-key` — move between panes  
