# Python API for Ratchetron
Before running this, upload the SPRX in sprx/ to anywhere on your system and load it through webMAN on the "PS3MAPI" 
page under the "VSH Plugins" section. Any slot is fine. Don't try to unload it, it'll just lock up webMAN with 
"Server busy" until you restart the PS3.

## Sample code
```python
import time

api = Ratchetron(sys.argv[1])
if api.connect():
    print("Connected!")
    api.notify("Notification that shows up on the screen.")

    pid_list = api.get_pid_list()

    print(f"pids: {pid_list}")

    title_id = api.get_game_title_id()

    print(f"Game Title ID: {title_id}")
    print(f"Game Title: {api.get_game_title()}")

    if title_id == "NPEA00385":
        print("Ratchet & Clank 1 detected, setting and then showing bolt count in real time as demo.")
        
        api.memory_set(pid_list[2], 0x969CA0, 4, bytearray((420420420).to_bytes(4, "big")))
        
        while True:
            time.sleep(0.01666667)
            print("\rBolts: " + str(int.from_bytes(api.memory_get(pid_list[2], 0x969CA0, 4), "big")), end="")
````