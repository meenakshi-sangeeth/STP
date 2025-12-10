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

HOST = "filtermaze.2025.ctfcompetition.com"
PORT = 1337

def get_pow_token(banner: str) -> str:
  
    m = re.search(r"solve\s+(s\.[^\s]+)", banner)
    if not m:
        raise ValueError("Could not find PoW token")
    return m.group(1)


def solve_pow(token: str) -> str:
    cmd = f"python3 <(curl -sSL https://goo.gle/kctf-pow) solve {token}"
    out = subprocess.check_output(["bash", "-lc", cmd], text=True)
    
    for line in reversed(out.splitlines()):
        m = re.search(r"(s\.[A-Za-z0-9+/=]+)", line)
        if m:
            return m.group(1)
    raise ValueError("Could not solve PoW")

def solve_maze(r):
   
    with open("graph.json") as f:
        graph_raw = json.load(f)
    graph = {int(k): v for k, v in graph_raw.items()}

    path = [0]
    print("Starting path finding from node 0")

    while True:
        last = path[-1]
        neighbors = graph[last]
        
        for nxt in neighbors:
            if nxt in path:
                continue  
            candidate = path + [nxt]
            cmd = {"command": "check_path", "segment": candidate}
            r.sendline(json.dumps(cmd).encode())
            resp_line = r.recvline().decode().strip()
            resp = json.loads(resp_line)
            status = resp.get("status")

            if status == "valid_prefix":
                path.append(nxt)
                print(f"Extended path to length {len(path)}: ...{path[-3:]}")
                break
            elif status == "path_complete":
                full_path = candidate
                err_mags = resp["lwe_error_magnitudes"]
                return full_path, err_mags

def main():
    print("Connecting to server")
    r = remote(HOST, PORT)
    banner = r.recvuntil(b"Solution?").decode()
    token = get_pow_token(banner)
    print(f"Solving PoW: {token}")
    solution = solve_pow(token)
    r.sendline(solution.encode())
    for _ in range(4):
        line = r.recvline(timeout=2)
        if not line:
            break
    path, err_mags = solve_maze(r)
    r.close()    
    print("\nFound the secret path:")
    print(f"Path: {path}")
    print(f"\nReceived error magnitudes (first 10): {err_mags[:10]}")
    print(f"Total error values: {len(err_mags)}")
    
    with open("error_magnitudes.json", "w") as f:
        json.dump(err_mags, f)

if __name__ == "__main__":
    main()
```





```
