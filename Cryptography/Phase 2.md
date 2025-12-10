## Challenge Description

I have left a key to the chest at the end of the maze.

Connect to the instance at : `nc filtermaze.2025.ctfcompetition.com 1337`

## Solution

This is a challenge that combines graphy theory with lattice based cryptography (LWE - cryptographic problem where we have the equation `b = A*s + e (mod q)` and need to find the secret `s`)

When we connect to the server, we can
- Check if a path through the maze is correct (check_path)
- Submit the secret key to get the flag (get_flag)

The graph structure and LWE parameters matrix A, vector b and modulus q are given but not the Hamiltonian path or the error vector e. So first we need to find them

```bash
import json
import re
import subprocess
from pwn import remote

SERVER = "filtermaze.2025.ctfcompetition.com"
SERVER_PORT = 1337


def extract_pow_challenge(text: str) -> str:
    match = re.search(r"solve\s+(s\.[^\s]+)", text)
    if not match:
        raise RuntimeError("PoW challenge not found")
    return match.group(1)


def complete_pow(challenge: str) -> str:
    command = (
        f"python3 <(curl -sSL https://goo.gle/kctf-pow) "
        f"solve {challenge}"
    )
    output = subprocess.check_output(["bash", "-lc", command], text=True)

    for line in reversed(output.splitlines()):
        m = re.search(r"(s\.[A-Za-z0-9+/=]+)", line)
        if m:
            return m.group(1)

    raise RuntimeError("Failed to extract PoW solution")


def traverse_maze(conn):
    with open("graph.json") as f:
        raw_graph = json.load(f)

    maze = {int(node): edges for node, edges in raw_graph.items()}
    current_path = [0]

    print("Maze traversal initiated at node 0")

    while True:
        current_node = current_path[-1]
        neighbors = maze[current_node]

        for next_node in neighbors:
            if next_node in current_path:
                continue

            trial_path = current_path + [next_node]
            payload = {
                "command": "check_path",
                "segment": trial_path
            }

            conn.sendline(json.dumps(payload).encode())
            response = json.loads(conn.recvline().decode().strip())
            state = response.get("status")

            if state == "valid_prefix":
                current_path.append(next_node)
                print(f"Path extended → length {len(current_path)}")
                break

            if state == "path_complete":
                return trial_path, response["lwe_error_magnitudes"]


def run():
    print("Opening connection to challenge server")
    io = remote(SERVER, SERVER_PORT)

    banner = io.recvuntil(b"Solution?").decode()
    challenge_token = extract_pow_challenge(banner)

    print(f"PoW challenge received: {challenge_token}")
    pow_answer = complete_pow(challenge_token)
    io.sendline(pow_answer.encode())

    # clear remaining server messages
    for _ in range(4):
        io.recvline(timeout=2)

    solution_path, errors = traverse_maze(io)
    io.close()

    print("\nMaze solved")
    print(f"Final path: {solution_path}")
    print(f"Error magnitudes : {errors[:10]}")
    print(f"Total errors: {len(errors)}")

    with open("error_magnitudes.json", "w") as f:
        json.dump(errors, f)


if __name__ == "__main__":
    run()

```





```
