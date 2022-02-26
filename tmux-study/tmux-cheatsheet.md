## Command Line Options

### Create New Session
    tmux new -A -s session_name

### Attach to Session
    tmux attach -t session_name

### Show Active Sessions
    tmux ls

## Control-B Commands

### Control-B, then:
- D -> Detach Session
- C -> Create Window
- N -> Next Window
- P -> Previous Window
- 0-9 -> Go to Window
- % -> Split Horizontal
- " -> Split Vertical
- Arrows -> Switch Pane
- O -> Next Pane
- X -> Close Pane/Window
